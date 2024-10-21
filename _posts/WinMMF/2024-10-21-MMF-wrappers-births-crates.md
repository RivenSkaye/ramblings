---
layout: post
author: Riven Skaye
tags: [ipc, windows, shared-mem, winmmf, rust, tech]
series: WinMMF
category: winmmf
title: WinMMF - Making a Rusty wrapper
---

# Wrappers, births and crates

As I wrote [back in may](./mmf-primer), then [continued a few days later and stalled](./mmf-getting-started) for like five months, it's proven possible to use Rust to access Named Shared Memory in Windows. And not just that, I have [a very real crate](https://crates.io/crates/winmmf) that provides a full implementation of it in a working state. It's time to ~~spread more salt~~ talk further development after a few months of radio silence!

## And now we're wrapping MMFs with Rust

So, I'm trying to share large chunks of memory. And I somehow have to ensure several hard things. I'll need to make sure nobody reads while I'm writing, and the other way around. I'll need to make sure that locking is atomic, too. No sense in me taking the lock if you can't see that when you do the same. Oh, and I need to find a way to share that lock to _everyone else that uses the MMF_. Yeah okay, we can do that I think? At least, that's how it felt at first. And then [my buddy Joel](https://github.com/xacrimon) pointed out [reordering risks](https://github.com/RivenSkaye/WinMMF-rs/issues/5) and [some lock safety things](https://github.com/RivenSkaye/WinMMF-rs/issues/6). Though granted, we did discuss some stuff and he gave me some pointers on how to make this code _actually_ safe in a multiprocess environment. Thanks Joel!

The design itself was simple though. If we _really_ need everyone using the MMF to see that lock, why don't we store it in the MMF? Windows guarantees [4k alignment](https://devblogs.microsoft.com/oldnewthing/20210510-00/?p=105200) (which is always a multiple of the pointer size). For more information, look at the `MapViewOfFile*` family of functions, specifically [3 lets you specify a base address](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-mapviewoffile3) and that even ensures alignment _to 64k_. We use the base one without numbers though, as we don't care what address the view lives at. We just care about alignment and size. Next up is the issue of atomicity, to ensure changes in the lock are visible to everyone else. Windows guarantees [ops on 32-bit variables are atomic](https://learn.microsoft.com/en-us/windows/win32/sync/interlocked-variable-access) everywhere, with 64-bit operations only being guaranteed atomic for 64-bit Windows runing 64-bit applications. As I can't predict what targets people will compile to, and I'd like this to work on both x86 and AMD64 for better portability across different situations, our best option is a 32-bit atomic int. I could do `usize`, but then we'd have variable lock sizes depending on the platform and there's no benefit to doing that whatsoever. So, someone requests a mapping of N bytes. We push on an extra 4 that they'll never see (but it is documented) and we slide the lock in. This gives us a shared lock, made of atomic components, and anyone opening the mapping can see it. The thing is simple and naive, it spins when requested, uses Rust's beautiful `Result`s, and best of all, it doesn't rely on external components. And all it takes is a little bit of pointer shuffling magic!

Now all that could still go wrong was the OS call part of this. And as you might be able to tell from the couple of links to MS resources here and in the previous installments, that also means _dealing with Windows' exception model. Now this is hard to do right, as there is no support for it in GCC and limited support in LLVM behind the `-fms-compat` flag. GCC does the only thing it really can here, which is unwinding. LLVM has spent some time reimplementing the `__try` macro from MSVC, but this isn't stable or supported yet. Luckily, however, a tiny C approach with a Rust wrapper exists named [microseh](https://github.com/sonodima/microseh). And just when I was ready to start pushing somewhat stable versions to crates.io, this tiny library forced me to rewrite hald my code. In a good way, because I was using an ugly macro because `try_seh` didn't propagate the closure's return value. It does now, so give it a whirl next time you need to do things where Windows exceptions might happen!

## the birth of a crate

This is also just about the time when I'd need to ensure I wouldn't hit a wall trying to publish this crate. [publishing to crates.io](https://crates.io/crates/winmmf) when all you have is a crate nobody has ever heard of takes a fair bit of [hubris](https://www.merriam-webster.com/dictionary/hubris). Nobody knows about your project, and yet you're claiming a name for eternity. Oh and also you should discourage premature use through the README only, as crates aren't allowed to publish if they don't build. Though I do suppose that prevents practices like name squatting. &lt;padme&gt;: It prevents name squatting right?  
That aside, it does send a message to anyone who stumbles across your crate. And the frequent version bumps in the following days when you're stabilizing things and improving the recipe [so your dogfood is edible](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) are a clear sign that you're serious about keeping this thing working for the forseeable future.

So now you have a crate, it's publicly available, and what's next? Well, become your own first user! And without caring for who else uses it, see what the future holds for your project. I have no idea if this will take off or not. But what I did do, every step of the way, was look at the API and make sure it was something I'd use _if someone else wrote it_. The driving force there was to make sure that the API was nice to use. It has to reflect the way things work with the Win32 API because that's literally what we're working with. But it also has to feel a bit Rusty and it needs to lend itself to custom implementations and people creating different fronts and interfaces to use this. With that design goal in mind, I tried to split up the bits and pieces in a way that allows people that freedom. [Let me know if you think something should change in that regard.](mailto:riven@tae.moe)

As for the future of my shiny new crate, I have no idea what's gonna happen there. It might expand, it might even attract a crowd, or it stays as-is and dies in obscurity with me. Regardless, I'm proud of what I made, and of the fact I got it to work the way I want. I'm also happy that the implementation phase of this thing wasn't as much work after getting a PoC to run. I've started adding unit tests and I'm looking into expanding those to cover a whole range of cases. But for now, I can continue the mission for implementing camera callbacks writing into the MMF. But the WinMMF series itself is hereby officially over. I'll continue on with other Windows pains until this project is done, but they're still pretty related as far as the whole camera multiplexing situation goes.