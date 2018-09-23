---
title: "Rust HACL"
date: 2018-09-22
type: "blog_post"
draft: false
---

Having chosen rust as the language to implement packnback with, I needed a high
quality implementation of the Nacl cryptographic api.

Luckily, I had previously read about the [High-Assurance Cryptographic Library](https://github.com/project-everest/hacl-star)
which uses the F* language and formal verification to produce a C library and thought I could use this as an opportunity to learn
about the Rust C ffi. 

[Here](https://github.com/packnback/packnback/rust/haclstar) is the first bit of code. *After* finishing this, I found someone
else had the [same idea](https://github.com/quininer/rust-hacl-star), but for now I want to maintain control over this
dependency and it's wrapper.

I made some small additions to the C api while playing with rust:

- Rust naming conventions for types.
- Boxing (unique pointers) secret keys to minimize accidental possible copies in memory.
- Wiping secret keys on drop.
- Misuse resistant type wrappers around Secret/Public keys.
- Assertions around api preconditions.
