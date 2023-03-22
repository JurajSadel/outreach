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

**Why Rust?**

Rust's focus on memory safety and thread safety aligns with goal of producing reliable and secure software. Also, Rust's support for concurrency and parallelism is particularly relevant for embedded development, where efficient use of resources is critical. Rust's growing popularity and ecosystem make it an attractive option for developers, especially those who are looking for a modern language that is both efficient and safe. These are the main reasons why is Rust becoming an increasingly popular choice not only in embedded development especially for projects that prioritize **safety**, **security** and **reliability**. Let's briefly talk about each in more detail.
TODO: mention first contributors, ask them if I can mention them, ask Ivan

### Advantages of Rust (compared with C and C++)

- Memory safety: Rust offers strong memory safety guarantees through its **ownership** and **borrowing** system which is very helpful in preventing common memory-related bugs like **null pointer dereferences** or **buffer overflow**, for example. In other words, Rust guarantees **memory safety** at compile time through ownership and borrowing system. This is specially important in embedded developement where memory/resources limitations can make such bugs more challenging to detect and resolve.
- Concurrency: Rust provides excellent support for zero-cost abstractions and safe concurrency and multi-t
hreading, with a built-in **async/await** syntax and a powerful types system that prevents common concurrency bugs like data races. This can make it easier to write safe and effecient concurrent code not only in embedded systems.
- Performance: Rust is designed for high performance and can go toe-to-toe with C and C++ in performance measures while still providing strong memory safety guarantees and concurrency support.
- Readability: Rust's syntax is designed to be more readable and less error-prone than C and C++, with features like pattern matching, type inference, and functional programming constructs. This can make it easier to write and maintain code, espresially for larger and more complex projects.
- Growing ecosystem: Rust has a growing ecosystem of libraries (crates), tools and resources for (not only) embedded developement, which can make it easier to get started with Rust and find necessary support and resources for a particular project.

### Disadvantages of Rust (compared with C and C++)

On the other hand, Rust is not perfect language and has also some disadvantages over other programming languages (not only C and C++).
- Learning curve: Rust has a steeper learning curve than many programming languages, including C and C++. It's unique features, such as already mentioned ownership and borrowing, may take some time to understand and get used to and therefore are more challenging to get started with Rust.
- Compilation time: Rust's advanced type system and borrow checker can result in longer compilation times compared to other languages, especially for large projects.
- Tooling: While Rust's ecosystem is growing rapidly, it may not yet have the same level of tooling support as more established programming languages. For example, C and C++ have been around for decades and have a vast codebase. This can make it more challenging to find and use the right tools for a particular project.
- Lack of low-level control: Rust's safety features can sometimes limit low-level control to C and C++. This can make it more challenging to perform certain low-level optimizations or interact with hardware directly, but it's definitely possible.
- Community size: Rust is still relatively new programming language compared to more established languages like C and C++, which means that it may have a smaller community of developers and contributors, fewer resources, libraries and tools.

Overall, Rust offers many advantages over traditional embedded development languages like C and C++, including memory safety, concurrency support, performance, code readability, and a growing ecosystem. As a result, Rust is becoming an increasingly popular choice for embedded developement, especially for projects that prioritize safety, security, and reliability. The disadvantages of Rust compared to C and C++ tend to be related to Rust's relative newness as a language and its unique features. However, many developers find that Rust's advantages make it a compelling choice for certain projects.

### Should I switch from C to Rust?

If you are starting a new project or a task where memory safety or concurrency is required, it may be worth considering moving from C to Rust. However, if your project is already well-established and functional in C, the benefits of switching to Rust may not outweigh the costs of rewritting and retesting your whole codebase. In this case you can consider to keep the current C codebase and start writing adn adding new features, modules and functionality in Rust - it is relatively simple to call C functions from Rust code. In the end, the final decision to move from C to Rust should be based on a careful evaluation of your specific needs and the trade-offs involved.

### Why Espressif started with Rust?

First of all it were the community members who started playing around Rust and the ESP chips and making it possible to compile the code, flash the code and actually use it for something useful and interesting.

Why espressif started with Rust, should I move from C to Rust etc
Pros and cons of Rust, some missing features in core language
Its good to inform users about it (mention tracking issues for example)
Developers try to convince other developers

## What is STD

In Rust, `std` refers to the standard library, which can be seen as a collection of modules and types that are included with every Rust installation. The `std` provides set of multiple functionality for building Rust programs, including **data structures**, **networking**, **mutexes and other synchronization primitives**, **input/output** and more.

With `std` approach you can use the functionality from the C-based developement framework called `esp-idf` because `esp-idf` provides a `newlib` environment that is "powerful" enough to build the Rust standard library on top of it. In other words, with `std` approach we use the `esp-idf` as an operating system and buld Rust application on top of it. In this way we can use all the standard library features listed above and also already implemented C functionality from esp-idf API.

We can see how to blink with an LED with `std` approach:


TODO: add some comments about code as well, just for the first one, dont explain why concrete imports, just say few words about use for example

```rust
// Import peripherals we will use in the example
use esp_idf_hal::delay::FreeRtos;
use esp_idf_hal::gpio::*;
use esp_idf_hal::peripherals::Peripherals;

// Start of our main function i.e entry point of our example
fn main() -> anyhow::Result<()> {
    // Apply some required ESP-IDF patches
    esp_idf_sys::link_patches();

    // Initialize all required peripherals
    let peripherals = Peripherals::take().unwrap();

    // Create led object as GPIO4 output pin
    let mut led = PinDriver::output(peripherals.pins.gpio4)?;

    // Infinite loop where we are constantly turning ON and OFF the LED every 500ms
    loop {
        led.set_high()?;
        // we are sleeping here to make sure the watchdog isn't triggered
        FreeRtos::delay_ms(1000);

        led.set_low()?;
        FreeRtos::delay_ms(1000);
    }
}
```

### When you might want to use `std`

- Rich functionality: If your embedded system requires lots of functionality like support for networking protocols, file I/O, or complex data structures, you will likely want to use `std` approach because `std` libraries provide a wide range of funtionality that can be used to build complex applications relatively quickly and efficiently
- Portability: The `std` library provides a standardized set of APIs that can be used across different platforms and architectures, making it easier to write code that is portable and reusable.
- Rapid development: The `std` library provides a rich set of functionality that can be used to build applications quickly and efficiently, without worrying about low-level details.


### What is possible in STD

What is possible in STD sounds like subchapter and not chapter, better is `What is STD` for example
Always a good to show some demo
Focus on basics

Briefly explain what is STD approach
Using ESP-IDF as an operating system because of `newlib` - this allows us using features from standard library that people are used to when developing Desktop applications such `threads`, `multiple colections` etc ...
generating bindings to the ESP-IDF C functions - we can basically do everything what the ESP-IDF can do (`esp-idf-sys` and `embuild` repos) these are responsible for allowing the usage of ESP-IDF functions
Other important repos `esp-idf-hal` adn `esp-idf-svc` these are the "real" API with HW peripherals and services

What chips are supported?

As a demo we can use Ivan Markov's `std-demo`

## What is NO_STD

When a Rust program is compiled with the `no_std` attribute, it means that the program will not have access to certain features (some are listed in STD chapter). `no_std` programs rely on set of `core` language features that are available in all Rust environments, for example data types, control structures or low-level memory management. This approach is useful for embedded programming where memory usage is often constrained and low-level control over hardware is required.

### When you might want to use `no_std`

- Small memory footprint: If your embedded system has limited resources and needs to have a small memory footprint, you will likely want to use `no_std` because `std` features add significant amount of final binary size and compilation time.
- Direct hardware control: If your embedded system requires more direct control over the hardware, such as low-level device drivers or access to soecialized hardware features you will likely want to use `no_std` because `std`adds abstractions that can make it harder to interact directly with hardware.
- Real-time constraints or time critical applications: If your embedded system requires real-time performance or low-latency response times because `std` can introduce unpredictable delays and overhead that can affect real-time performance.
- Custom requirenments: `no_std` allows more customization and fine-grained control over the behavior of an application, which can be useful in specialized or non-standard environments.


### What is possible in NO_STD

TODO: add some comments about code as well, just for the first one, dont explain why concrete imports, just say few words about use for example
TODO: brief description, not reuired deeply, just important steps

```rust
#![no_std]
#![no_main]

// Import peripherals we will use in the example
use esp32c3_hal::{
    clock::ClockControl,
    gpio::IO,
    peripherals::Peripherals,
    prelude::*,
    timer::TimerGroup,
    Delay,
    Rtc,
};
use esp_backtrace as _;

// Set a starting point for program execution
// Because this is `no_std` program, we do not have a main function
#[entry]
fn main() -> ! {
    // Initialize all required peripherals
    let peripherals = Peripherals::take();
    let system = peripherals.SYSTEM.split();
    let clocks = ClockControl::boot_defaults(system.clock_control).freeze();

    // Disable the watchdog timers. For the ESP32-C3, this includes the Super WDT,
    // the RTC WDT, and the TIMG WDTs.
    let mut rtc = Rtc::new(peripherals.RTC_CNTL);
    let timer_group0 = TimerGroup::new(peripherals.TIMG0, &clocks);
    let mut wdt0 = timer_group0.wdt;
    let timer_group1 = TimerGroup::new(peripherals.TIMG1, &clocks);
    let mut wdt1 = timer_group1.wdt;

    rtc.swd.disable();
    rtc.rwdt.disable();
    wdt0.disable();
    wdt1.disable();

    // Set GPIO4 as an output, and set its state high initially.
    let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
    // Create led object as GPIO4 output pin
    let mut led = io.pins.gpio4.into_push_pull_output();

    // Turn on LED
    led.set_high().unwrap();

    // Initialize the Delay peripheral, and use it to toggle the LED state in a
    // loop.
    let mut delay = Delay::new(&clocks);

    // Infinite loop where we are constantly turning ON and OFF the LED every 500ms
    loop {
        led.toggle().unwrap();
        delay.delay_ms(500u32);
    }
}
```

TODO: https://espressifsystems.sharepoint.com/sites/Documentation/SitePages/Content-Functions.aspx?OR=Teams-HL&CT=1678267382988&clickparams=eyJBcHBOYW1lIjoiVGVhbXMtRGVza3RvcCIsIkFwcFZlcnNpb24iOiIyNy8yMzAyMDUwMTQyMSIsIkhhc0ZlZGVyYXRlZFVzZXIiOmZhbHNlfQ%3D%3D

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