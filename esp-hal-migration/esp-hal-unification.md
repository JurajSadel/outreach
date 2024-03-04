### Unification of the esp-hal 

From its inception, the [esp-hal](https://github.com/esp-rs/esp-hal) repository has contained separate packages for each supported device. The thought early on was that there would be enough device-specific code to justify putting it in these packages rather than esp-hal-common. As time went on and additional devices were supported, it has been increasingly clear that this is not really the case. Having separate package for each ESP32 device were making realese and maintainance of the `esp-hal` pretty challenging due to having huge amount of duplicated example code and linker scripts. That's why we decided to unify all the `espXX-hal` packages, and thanks to [@jessebraham](https://github.com/jessebraham)'s hard work and dedication, we were successful of doing so! 

To simplify usage of the unified [esp-hal], we added the [xtask] automation to help building examples, package, and documentation. Also, `xtask` helps you to track, what examples could be run on which chip target. 

Let's look at very simple example (basic example generated with [esp-template](https://github.com/esp-rs/esp-template)) where we can see, what changes need to be done to migrate to unified `esp-hal`. We will use `ESP32-C3` chip.

- We need to change the name of the dependency crate in `Cargo.toml`
    So instead of having something like (yours can look slightly different, different chip or using some additional features)
    ```toml
    [dependencies]
    esp32c3-hal = "0.15.0"
    ```

    you need to use

    ```toml
    esp-hal = { package = "esp-hal", git = "https://github.com/esp-rs/esp-hal", features = ["esp32c3"] }
    ```

- We need to change the import in our source code
    So instead of

    ```rust
    use esp32c3_hal::{...};
    ```

    you need to use

    ```rust
    use esp_hal::{...};
    ```

<!-- After unification, [esp-hal] now contains the [esp-hal], [esp-hal-procmacros], [esp-hal-smartled] packages -->

[esp-hal]: https://github.com/esp-rs/esp-hal/tree/main/esp-hal
[esp-hal-procmacros]: https://github.com/esp-rs/esp-hal/tree/main/esp-hal-procmacros
[esp-hal-smartled]: https://github.com/esp-rs/esp-hal/tree/main/esp-hal-smartled
[esp-lp-hal]: https://github.com/esp-rs/esp-hal/tree/main/esp-lp-hal
[esp-riscv-rt]: https://github.com/esp-rs/esp-hal/tree/main/esp-riscv-rt
[examples]: https://github.com/esp-rs/esp-hal/tree/main/examples
[xtask]: https://github.com/esp-rs/esp-hal/tree/main/xtask