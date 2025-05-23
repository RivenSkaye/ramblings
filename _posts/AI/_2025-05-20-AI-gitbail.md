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
immediately forward all pushes to mirrors, so that any time I want to `cargo publish` I don't have to worry about syncing.

There are even some benefits. For example, I could make this blog support comments. And I could set up a cronjob to build
and push relevant things like profile info locally, which makes it more flexible by letting me parse commit dates.

Unfortunately though, it doesn't send a message to the place that should get it the most. GitHub is being used to [encourage the meme](https://github.blog/ai-and-ml/vibe-coding-your-roadmap-to-becoming-an-ai-developer/)
They have been at it for [over a year now](https://github.blog/developer-skills/career-growth/5-tips-to-supercharge-your-developer-career-in-2024/)
and they keep pushing to make the experience worse for anyone with halfway decent programming skills.
