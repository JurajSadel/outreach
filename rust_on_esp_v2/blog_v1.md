# Table of Contents

1. [Introduction](#Introduction)

3. [What is STD](#STD)
4. [What is NO_STD](#NO_STD)
5. [What is async](#Async)
6. [Conclusion](#Conclusion)
2. [Getting started](#Getting_Started)




## Introduction

Mention what is the motivation/goal of this post
The main motivation is to briefly show what is currently possible with Rust and the ESP chips

Here we should mention the most important and the biggets update we have made since the first Scott's post - that's basically everything because the post was published in September 2019


## Getting started Why Rust ?


Here we should briefly explain whole `esp-rs` organization - during the first post only few repos were there
Also briefly mention our tooling i.e **how to setup project** - mention **espup**, **templates**, **book**, **STD_training**
How to build/flash chip **espflash**
The question is important/relevant is this to this post?


Its not connected, we can get getting started after conclusion

insert the most hard questions, why rust? or similar
Why espressif started with Rust, should I move from C to Rust etc
Pros and cons of Rust, some missing features in core language
Its good to inform users about it (mention tracking issues for example)
Developers try to convince other developers


## What is possible in STD

What is possible in STD sounds like subchapter and not chapter, better is `What is STD` for example
Always a good to show some demo
Focus on basics

Briefly explain what is STD approach
Using ESP-IDF as an operating system because of `newlib` - this allows us using features from standard library that people are used to when developing Desktop applications such `threads`, `multiple colections` etc ...
generating bindings to the ESP-IDF C functions - we can basically do everything what the ESP-IDF can do (`esp-idf-sys` and `embuild` repos) these are responsible for allowing the usage of ESP-IDF functions
Other important repos `esp-idf-hal` adn `esp-idf-svc` these are the "real" API with HW peripherals and services

What chips are supported?

As a demo we can use Ivan Markov's `std-demo`
## What is possible in NO_STD

Briefly explain what is NO_STD approach - we don't have operating system i.e the ESP-IDF and everything is running on `bare-metal`
Optional: should we mention svds here?
Important repos `esp-hal`, `esp-wifi`

What chips are supported?
What peripherals (`esp-hal`)? Big majority of peripherals
What "features" (`esp-wifi`)? WiFi, BLE, esp-now

Add a simple demo - use some existing example from mentioned repos or create something new as a combination of both?

We are supporting all chipts that are possible to buy (ESP32, ESP32S2, ESP32S3, ESP32C2, ESP32C3) ESP32C6 is very close to being merge into main, H2 will be pick up after C6 is in the main branch

We can use a tracking issue wuth supported chips/peripherals

Why no_std makes sense? What we should consider before choosing STD vs NO_STD
Highlight some ideas, maybe critical safety applications

We can stop here with article

## Async

Briefly explain async + embassy

This topic will probably change the most while the post is not published

### NO_STD

Currently there isn't that much supported, we do have `GPIO`, `TIMG`, `SYSTIMER` and `SPI` supported with bunch of helper traits

## Conclusion

What are future plans, what are we working on right now?
Mention **802.15.4**, **ULP support (sleep modes)**, adding H2 support, adding more Async support

Note: How to start with Rust from getting started instead
next articla can be linked to how to get started and description all the basic steps can be mentioned here

## Miscellaneous

LLVM xtensa patches were upstreamed (not all of them yet, but progress)
Other useful repos `esp-println`, `esp-backtrace`
Mention Matrix channel and community meetings