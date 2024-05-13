---
layout: post
author: Riven Skaye
tags: [intro, ipc, windows, shared_mem, winmmf, rust, tech]
---
# Users, bugs, and technical limitations

## A primer on how WinMMF-rs came to be

So one day at work, someone from production rang up to my office to report an issue with something I wrote. A strange crash every time they scanned an order ID tag with no error message. This was properly weird to me, as the logs seemed to be clean and, though reproducible on the target system, did not occur on my machine.

Note: shipping my machine would _not_ have fixed this. Shipping a different user might have.

Now before people start wondering if it really isn't just me, I should give some context on the application in question. It's a graphical app, written in C# using the [AvaloniaUI framework](https://avaloniaui.net/). The framework allows for a global exception handler to be registered, which I use to log whatever has somehow made it past the rigorous error handling in other places to cover strange corner cases. The code then resumes through the normal exit steps of teardown and cleanup. The logs? They looked like a normal program exit. There was no error provided and I had no idea what was even up. Considering everything worked on my machine (which at work is not Arch, btw) I just rebooted and all was well again.

Fast forward a few weeks and I'd forgotten all about it by now. It was a one-off issue that didn't show up again. Blame Windows and carry on, right? Except I got a call about it happening again. So I went down there and looked into things, and then the conclusion was both simple _and_ weird. Yes, the system was throwing an exception that went unhandled. Because it was something that I couldn't handle. Turns out the library I used for the camera was the cause of an unhandled exception in C++ space. It was handling all but one possible exception. That's right, only one application can use a USB camera at a time. And when two applications that shouldn't be used at the same time are grabbing the same USB camera on the PC, you can ~~blame the user~~ expect weird shit!

## So, what happened?

In this case, we had two applications opening the camera. The user, just doing their work when they got back from an interruption, was confronted by the later handle being erroneous and crashing the app. That led to two applications opening a handle to an exclusive hardware resource, causing a low level exception that just caused a mostly silent nuke. Poking around in the system event log led me to the exception in question, which clearly happened in "unmanaged code" as MS puts it, which can cause silent crashes.

If you want more examples of this, try to trigger an OS level exception from Rust code built with GCC, targeting i686. On Windows, OS exceptions are passed through Structured Exception Handling. But this same infrastructure is also used for some system message passing akin to Unix signals, and not all of them are considered fatal. Not assigning handlers and doing proper recovery on those means the OS will just send your process into the Shadow Realm. There's some other ways to try and save your hide from within the dotnet CLR, but for that to be useful you should be expecting the errors. So now I handle these errors properly, but this feels like an improper solution. We know we need the cameras, and we also know _nothing else_ should need them while the applications use them. So I started thinking about alternate solutions!

Searching the web for ways to multiplex the camera quickly led to either using OBS (lmao the PCs they use are not fit for that) or basically rolling my own solution involving non-sequential, safely ignored IPC. Either that or writing my own driver, but that is **definitely** not something I want to do here for the love of all that is good and not segfaulting.

Note: I'm being paid to write something maintainable, not something they can throw away when they finally need to replace these machines.

## IPC? Just `mmap`/`shmem` or something

Well yes, on anything Unix-like that would work perfectly. But on Windows we're stuck to different APIs and possibilities and permissions. Now I have to admit that the docs on [named shared memory](https://learn.microsoft.com/en-us/windows/win32/memory/creating-named-shared-memory) are fine and crystal clear, if you can find them. That's a challenge. But alas, that is also the only real solution we have here that doesn't have one of the dealbreakers for the situation of working with live camera footage at random.

- [anonymous pipes](https://learn.microsoft.com/en-us/windows/win32/ipc/anonymous-pipes) are sequential and we'd need to open a new one every time we want a frame or the camera feed. They also don't really multiplex.
- [Named pipes](https://learn.microsoft.com/en-us/windows/win32/ipc/named-pipes) multiplex just fine (if you have a language interface that supports it). But they're still sequential. And they live in the NPFS (Named Pipe File System, a virtual FS) and data comes over the TCP stack. That's right, they just read and write over localhost.
- [RPC messages](https://learn.microsoft.com/en-us/windows/win32/rpc/how-rpc-works) are most suitable for small data. Not full video frames of 1920\*1080\*8\*3 bits each. Oh and it's also over the TCP stack.
- Anonymous handles to shared memory require all process to duplicate handles and you can only share them to children. I can't write a [~~SomethingManagerApp~~](https://blog.codinghorror.com/i-shall-call-it-somethingmanager/) common ancestor just to share a camera handle when users might use some other app that grabs the camera at some point.

Named Shared Memory is nice, but the only way to share it without starting everything from one parent application is to allocate it in the global namespace, for which you should run with [Service permissions or higher](https://learn.microsoft.com/en-us/windows/win32/secauthz/privilege-constants) (yes, you need to run as admin or create an account with this permission to debug this as IPC mechanism). It also requires first mapping a region of the desired size with the invalid handle as the file. This maps your filename to a memory region that's only backed by the pagefile, and that can be opened by everyone else at any time they wish.

That's right - I've decided to get going on a Rust implementation of Memory-Mapped Files, also known as Named Shared Memory! With the [`windows-rs` crate](https://crates.io/crates/windows) providing a fair amount of wrappers over the win32 APIs in varying levels of ergonomic, and crates like [Mullvad's windows-service wrapper](https://crates.io/crates/windows-service) to make sure we have a startup service to be the first to claim the camera, this is looking to be a very doable endavour in a fun language. And as I decided to start blogging in the midst of this all, why not make creating this a series of posts?
