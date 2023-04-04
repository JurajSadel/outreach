# Table of Contents

1. [Introduction](#Introduction)
2. [What is STD](#STD)
3. [What is NO_STD](#NO_STD)
4. [Conclusion](#Conclusion)

## Introduction

Mention what is the motivation/goal of this post
The main motivation is to briefly show what is currently possible with Rust and the ESP chips

Here we should mention the most important and the biggest update we have made since the first Scott's post - that's basically everything because the post was published in September 2019

TODO:
- so many prgrammng languages, webm mobile ...
- one paragraph in Past years new languages are appearig to mrpve proramming experience
- second we can assume that thr reader is awre tha ew are fong to talk aut progrmamng lamgiage
- let readrs now we are going o talk about ptogramming language
- 20 years 20 languages were avalaible now we have 100+
- then when rust was initialy developed
- then rust in emvedded
- in emvedde we sont have many options like in web etc
- in embedded we have micropyhton, C, basic, haskell ...
- huge differences beteen we and emvedded -> this can show to the readers that rust will complementary ehat we have today
- C is not language for the young generation (to hoin embedded world, we can create somethme more tractive)
- how to motivte people to join emveded world

**Why Rust?**

Rust's focus on memory safety and thread safety aligns with goal of producing reliable and secure software. Also, Rust's support for concurrency and parallelism is particularly relevant for embedded development, where efficient use of resources is critical. Rust's growing popularity and ecosystem make it an attractive option for developers, especially those who are looking for a modern language that is both efficient and safe. These are the main reasons why is Rust becoming an increasingly popular choice not only in embedded development especially for projects that prioritize **safety**, **security** and **reliability**. Let's briefly talk about each in more detail.

### Advantages of Rust (compared with C and C++)

- Memory safety: Rust offers strong memory safety guarantees through its **ownership** and **borrowing** system which is very helpful in preventing common memory-related bugs like **null pointer dereferences** or **buffer overflow**, for example. In other words, Rust guarantees **memory safety** at compile time through ownership and borrowing system. This is specially important in embedded development where memory/resources limitations can make such bugs more challenging to detect and resolve.
- Concurrency: Rust provides excellent support for zero-cost abstractions and safe concurrency and multi-threading, with a built-in **async/await** syntax and a powerful types system that prevents common concurrency bugs like data races. This can make it easier to write safe and efficient concurrent code not only in embedded systems.
- Performance: Rust is designed for high performance and can go toe-to-toe with C and C++ in performance measures while still providing strong memory safety guarantees and concurrency support.
- Readability: Rust's syntax is designed to be more readable and less error-prone than C and C++, with features like pattern matching, type inference, and functional programming constructs. This can make it easier to write and maintain code, especially for larger and more complex projects.
- Growing ecosystem: Rust has a growing ecosystem of libraries (crates), tools and resources for (not only) embedded development, which can make it easier to get started with Rust and find necessary support and resources for a particular project.
- Package manager and build system Cargo: Cargo is an official tool that is included in the Rust programming language distribution and is used to automate the build, test and publish process together with creating a new project and managing project dependencies.
### Disadvantages of Rust (compared with C and C++)

On the other hand, Rust is not perfect language and has also some disadvantages over other programming languages (not only C and C++).
- Learning curve: Rust has a steeper learning curve than many programming languages, including C and C++. It's unique features, such as already mentioned ownership and borrowing, may take some time to understand and get used to and therefore are more challenging to get started with Rust.
- Compilation time: Rust's advanced type system and borrow checker can result in longer compilation times compared to other languages, especially for large projects.
- Tooling: While Rust's ecosystem is growing rapidly, it may not yet have the same level of tooling support as more established programming languages. For example, C and C++ have been around for decades and have a vast codebase. This can make it more challenging to find and use the right tools for a particular project.
- Lack of low-level control: Rust's safety features can sometimes limit low-level control to C and C++. This can make it more challenging to perform certain low-level optimizations or interact with hardware directly, but it's definitely possible.
- Community size: Rust is still relatively new programming language compared to more established languages like C and C++, which means that it may have a smaller community of developers and contributors, fewer resources, libraries and tools.

Overall, Rust offers many advantages over traditional embedded development languages like C and C++, including memory safety, concurrency support, performance, code readability, and a growing ecosystem. As a result, Rust is becoming an increasingly popular choice for embedded development, especially for projects that prioritize safety, security, and reliability. The disadvantages of Rust compared to C and C++ tend to be related to Rust's relative newness as a language and its unique features. However, many developers find that Rust's advantages make it a compelling choice for certain projects.

### Should I switch from C to Rust?

If you are starting a new project or a task where memory safety or concurrency is required, it may be worth considering moving from C to Rust. However, if your project is already well-established and functional in C, the benefits of switching to Rust may not outweigh the costs of rewriting and retesting your whole codebase. In this case you can consider to keep the current C codebase and start writing adn adding new features, modules and functionality in Rust - it is relatively simple to call C functions from Rust code. In the end, the final decision to move from C to Rust should be based on a careful evaluation of your specific needs and the trade-offs involved.


## What is STD

In Rust, `std` refers to the standard library, which can be seen as a collection of modules and types that are included with every Rust installation. The `std` provides set of multiple functionality for building Rust programs, including **data structures**, **networking**, **mutexes and other synchronization primitives**, **input/output** and more.

With `std` approach you can use the functionality from the C-based development framework called `esp-idf` because `esp-idf` provides a `newlib` environment that is "powerful" enough to build the Rust standard library on top of it. In other words, with `std` approach we use the `esp-idf` as an operating system and build Rust application on top of it. In this way we can use all the standard library features listed above and also already implemented C functionality from esp-idf API.

We can see how to blink with an LED with `std` approach:

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

- Rich functionality: If your embedded system requires lots of functionality like support for networking protocols, file I/O, or complex data structures, you will likely want to use `std` approach because `std` libraries provide a wide range of functionality that can be used to build complex applications relatively quickly and efficiently
- Portability: The `std` library provides a standardized set of APIs that can be used across different platforms and architectures, making it easier to write code that is portable and reusable.
- Rapid development: The `std` library provides a rich set of functionality that can be used to build applications quickly and efficiently, without worrying about low-level details.


### What is possible in STD

TODO: probably it will make more sense to focus on this in the next article

## What is NO_STD

When a Rust program is compiled with the `no_std` attribute, it means that the program will not have access to certain features (some are listed in STD chapter). `no_std` programs rely on set of `core` language features that are available in all Rust environments, for example data types, control structures or low-level memory management. This approach is useful for embedded programming where memory usage is often constrained and low-level control over hardware is required.

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

### When you might want to use `no_std`

- Small memory footprint: If your embedded system has limited resources and needs to have a small memory footprint, you will likely want to use `no_std` because `std` features add significant amount of final binary size and compilation time.
- Direct hardware control: If your embedded system requires more direct control over the hardware, such as low-level device drivers or access to specialized hardware features you will likely want to use `no_std` because `std`adds abstractions that can make it harder to interact directly with hardware.
- Real-time constraints or time critical applications: If your embedded system requires real-time performance or low-latency response times because `std` can introduce unpredictable delays and overhead that can affect real-time performance.
- Custom requirements: `no_std` allows more customization and fine-grained control over the behavior of an application, which can be useful in specialized or non-standard environments.


### What is possible in NO_STD

TODO: probably it will make more sense to focus on this in the next article

## Conclusion

In this article we went through of multiple interesting questions. We began with why should someone try or consider Rust programming language. Next, we compared advantages and disadvantages of Rust with C/C++ and mentioned some key things what should everyone consider before starting a new embedded project with interest in Rust or C/C++ and we summed it up with a few sentences if someone should switch to Rust. The second part of the article was focused on briefly describing `STD` and `NO_STD` with mentioning a differences where is `STD` better choice over `NO_STD` and vice versa. We also went through simple `STD` and `NO_STD` _blinky_ examples where we could see and compare the code structure and differences of each approach.




STATS:
Currently we have almost 1900 words