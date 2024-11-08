---
layout: post
author: Riven Skaye
tags: [ipc, windows, shared-mem, winmmf, rust, tech]
series: WinMMF
category: winmmf
title: WinMMF - Ending the Saga
---

# Weren't we done?

So the story seemed to be all wrapped up. That is, until I thanked the folks who were there as moral support. Remember [my buddy Joel](https://github.com/xacrimon)? Well, he noticed some small stuff and had some nits. Most of them justifiable, some of them being about errors that are less than perfect. Now he _is right_ of course. But it sucks when you're proud of your achievements and someone then has an "uhm akshually" moment after they say it looks good and give off an all-clear ...

## Minor flaws and annoying atomics

I was using [fetch\_update](doc link here) with some checking logic. Except that could actually leave locks somewhat unusable, or not guarantee the correctness of the value. You know, small stuff that's easily fixed with a few lines of code.
