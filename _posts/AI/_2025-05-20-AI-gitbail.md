---
layout: post
author: Riven Skaye
tags: [tech, personal, opinion]
series: AI
category: tech
title: AI - Bailing on GitHub
---

# Bailing on GitHub before you get buried in wasted time and AI slop

GitHub has been a good home for my projects for a long time. No traction, no visibility, and the few outside interactions were
with fellow humans doing code out of love for playing with it. Change is on the horizon though, and I don't want to even
risk getting [AI slop for issues](https://github.blog/changelog/2025-05-19-creating-issues-with-copilot-on-github-com-is-in-public-preview/).

I'm drawing a line at forcing me to accept AI in my workflow. I refuse to even acknowledge issues opened by Copilot and
similar AI tooling. If you hit a snag with software, you can type out your problem. Don't even need to look at the code,
know how it works, or anything. You just have to say "this is broken, pls fix" and I'll gladly help. But if you're not
gonna bother writing your own issue, then I'm not gonna bother helping you. And considering GH [doesn't let you block their AI](https://github.com/orgs/community/discussions/159749)
the only remaining solution is to just migrate away from the platform entirely.

## Preface - It's not just bashing people

No, this is not going to be baseless whining. It's also not going to be just bashing M$ every step of the way (though
admittedly I will because it's their recent decisions pushing me away from GH). There are actual reasons and arguments
for the choices I'm making and what I write here.  
That doesn't change the fact that this has been a long time coming; [all the way back since 2022](https://www.saverilawfirm.com/our-cases/github-copilot-intellectual-property-litigation)
when people went to sue M$ over Copilot and the amount of code it vomited out into the world. Code being used way
beyond what the licenses allow for, in incompatibly licensed projects, by people who don't have a clue because they
weren't even told as much.

Or perhaps it could even be pinned back to 2018, when [a lot of users were angry that M$ bought GH](https://qz.com/1295693/github-users-already-fuming-about-companys-sale-to-microsoft/)
and they already started questioning how [Embrace, Extend, Extinguish](https://en.wikipedia.org/wiki/Embrace%2C_extend%2C_and_extinguish)
would be applied to what was until then a safe platform. Suddenly the devs that were always calling users products felt
like they had walked into the same trap. GitHub seemed different before then. But now, they heard those words in the back of their minds again:

> If you're not paying for the product, you _are_ the product.

## The current situation

So as I mentioned before, as of 2025-05-19 GitHub lets you [generate AI slop issues](https://github.blog/changelog/2025-05-19-creating-issues-with-copilot-on-github-com-is-in-public-preview/)
as well as letting you [vibe code for 1337 H4X0R! rizz](https://github.blog/changelog/2025-05-19-github-copilot-coding-agent-in-public-preview/).
Now if you're wondering if you can't just go to the user page and block Copilot from there, heed [this warning to not try that](https://github.com/orgs/community/discussions/159749#discussioncomment-13201043) (even if you just want to block the thing)
as it will try and enroll you into the copilot program.

For anyone who doesn't know me, I despise AI. Neural Networks can be useful when trained for specific tasks. But LLMs
and generic models are nothing short of garbage. They hallucinate, ruin things, and provide misinformation at best.
I do ~~borderline piracy~~ video encoding as a hobby. Down or uploading is bad, that's the legally punishable bit. I'd
never claim to distribute protected material. And we notice a trend. NN """scalers""" (really, they're just shaders) tend
to destroy any textures and detail. They also like to invent detail. And that's fixable by clamping the output to jusy the
bits you really want out of it. LLMs and other AIs are no different. And what GH is doing now is telling randos to use their
huge models to bombard people with loads of slop. I won't stand for that, and neither should you!

## The Effect

This made me want to bail. As fast as possible. I'm set up to move the important stuff over, so let's have a look.

- I can move all "active" repos to [my codeberg](https://codeberg.org/RivenSkaye);
- I can [disable the issues feature on GitHub](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/disabling-issues)
  - ***__WARNING__***: this page offers you Copilot for Docs
- I can set the existing repos up as mirrors;
- I rent a VPS with [the very nice people at Netcup](https://netcup.com) that can host what I now use GitHub pages for;
- I could even put some of my stuff with the chads at [serv00, an actually free (small) VPS host](https://serv00.com)
  - At the time of writing they hit their user cap, but they open up every now and then

So what even keeps me on GitHub? Well, [crates.io](https://crates.io) and [docs.rs](https://docs.rs) require it. And
I have a few services linked with GH OAuth because I'm lazy and/or couldn't sign up otherwise. But that's not a big deal!
I can take the profile stuff on GH and use it to point people to my Codeberg instead. I can also set up the latter to
immediately forward all pushes to mirrors, should I need to.

Unfortunately though, it doesn't send a message to the place that should get it the most. GitHub is being used to [encourage the meme](https://github.blog/ai-and-ml/vibe-coding-your-roadmap-to-becoming-an-ai-developer/)
They have been at it for [over a year now](https://github.blog/developer-skills/career-growth/5-tips-to-supercharge-your-developer-career-in-2024/)
and they keep pushing to make the experience worse for anyone with halfway decent programming skills.
Sure, they've been doing AI garbage since well before that, but it's only recently that they've started aggressively pushing
everyone to use it to the detriment of the developer experience.

## Some paranoia

This also made me reconsider how much of a good thing the [Arctic Code Vault](https://archiveprogram.github.com/arctic-vault/)
really is. And whether or not that was just another angle for M$ to get millions of free training repositories for Copilot.
The legal action I linked earlier is about the same thing – their model spitting out verbatim code it shouldn't even be using that easily.
And they aren't the only ones. Your code is [probably in the BigCode Stack](https://huggingface.co/spaces/bigcode/in-the-stack).
A 67TB dataset that hasn't just been ignoring requests for exclusion, but one that has an opague opt-out process. They just include your
open source code whether you want it to be in there or not.

My paranoia here lies in the fact that I'm convinced there are way more of these datasets out there, and some are probably not
even listing sources. There is no way to determine who is or isn't training their AI on your code. Violating licenses has been
frowned upon for a long time. But it seems AI is paving the way to go back to the days of digital theft and profiting off of
other people's hard work without so much as a single line of attribution or compatible licensing. Basically if you're doing anything
open source, you're being used as free labor. Not with the dubious honour of some people using your product and being secretive
about it, but by breaking the trust and promises that make people proud to share their work.

The new GitHub Copilot for docs, issues, and PRs is making me worry about not just the safety of the platform, but about the future
of tech as an industry. If we can't trust companies to be upfront about where their code comes from, then who's to say they won't go
back to the days of excessive intimidation and silencing people that organizations like the FSF were founded to combat? (Let's disregard
the current state of affairs here.)

Don't even get me started on what I see happening in the new generations of programmers. We already laugh while feeling some internalized
horror over things like "vibecoding" and asking ChatGPT or other LLMs about code before reaching out to humans.
More than once have I had to show people the way things can very subtly (or extremely) break when using LLM code. Sure, your `/admin/`
endpoint is secured with auth. But that only works for `GET` requests to existing pages. Send a `POST` or `PUT` and your auth is bypassed
and any missing args are described. I dread this becoming more commonplace, and yet I see it happening all around. AI is becoming the
enshittification of programmers. companies now try and use it as a way to attract new employees. And people think you're nuts if you're not
on the hypetrain because it is so clearly the future. Well, it's not a future I'd want to be part of.

## What's next?

We as programmers need to send a message. And I personally see only two ways out. Neither of which is actually certain to help.
First of all, it's important to send a message one way or another. That message can come in one of two forms.
Option one is that we get together and start a petition or author [an open letter](https://en.m.wikipedia.org/wiki/Open_letter)
to try and stop this madness. I'm not saying all AI should stop. Just the part where they're shoving AI slop down everyone's throat.
That letter could be signed by everyone, and would help make it clear just how many people don't want this.
the other option is moving everything we can off of GitHub. It's a much more drastic measure, and would incur a lot of work for large projects.
But it would make it abundantly clear that current developments are not appreciated.

I don't see either of these happening any time soon. The discussions thread that started this is full of people threatening to move away, but few
are actually prepared to take action. Some people want to start a mudslinging contest, e.g. by bombarding M$ repositories with AI-generated issues,
but I feel like management will see that as a win. After all it both increases issue turnaround times (immediate close) and it bumps the amount of
Copilot users. The people making these decisions are so out of touch with reality that they just like the silly number going up.
Others are saying they'll leave if the issue isn't addressed. But responding with "get rekt, AI is staying" is technically addressing the discussion.
Some are even willing to leave if "more like this" happens. And I'm sure they have their keyboards ready to say the same thing next time. But I was also
pleasantly surprised with the amount of people that did actively start looking to get off of GitHub.

No matter which way the coin of public opinion drops though, I'm bailing. Repositories will become mirrors pretty soon. Issues and PRs will be turned off
entirely wherever possible on the GH side of things, and I'll look into automating updates, builds, and hosting of this blog elsewhere. By the time anybody
reads this, [the blog will already be mirrored](https://zwei.skaye.blog) and once that's stable I'll remove the Pages hosting and point the main URL to
my Netcup VPS. If you have any tips for the migration, including things like making workflow migrations easier, please give me a shout through [Codeberg](https://codeberg.org/RivenSkaye),
or [by email](mailto:riven@tae.moe).
