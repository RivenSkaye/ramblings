---
layout: post
author: Riven Skaye
tags: [ipc, windows, shared-mem, winmmf, rust, tech]
series: WinMMF
category: winmmf
title: WinMMF - Making a Rusty wrapper
---

# Wrapping MMFs with Rust

As I wrote [back in may](./mmf-primer), then [continued a few days later and stalled](./mmf-getting-started) for like five months, it's proven possible to use Rust to access Named Shared Memory in Windows. And not just that, I have [a very real crate](https://crates.io/crates/winmmf) that provides a full implementation of it in a working state. It's time to ~~spread more salt~~ talk further design and development after a few months of radio silence!

## the birth of a crate
