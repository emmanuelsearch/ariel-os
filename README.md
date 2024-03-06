# RIOT-rs
[![Build Status][build-badge]][build-info]
[![Documentation][doc-badge]][documentation-mdbook]
[![Matrix][matrix-badge]][matrix-link]

> Rust & RIOT combined for ergonomic embedded development

RIOT-rs is a project aiming to provide an adequate 
operating system for cybersecure, memory-safe, low-power IoT,
based on Rust from the ground up, and formal verification for critical modules. 
For a more verbose rant, see this
[manifesto](https://future-proof-iot.github.io/RIOT-rs/dev/manifesto.html).

Overall, RIOT-rs aims for a 'batteries-included' experience, on par
with [RIOT](https://github.com/RIOT-OS/RIOT): 
all the needed OS facilities and a high level of integration for 
the various libs, tools and toolchains required by low-power IoT application
development. Targets include various hardware based on 
32-bit microcontroller architectures (such as Cortex-M, RISC-V).
In particular, RIOT-rs aims for:

- **code portability** across all supported hardware, via consistent memory/energy efficient APIs;
- programming based either/both on **async & preemptive scheduling** paradigms, the latter using formally verified modules using [hax](https://hacspec.org/blog/posts/hax-v0-1/);
- **booting & update security**, via measured boot (tending towards [DICE](https://trustedcomputinggroup.org/work-groups/dice-architectures/) compliance) and secure software updates (tending towards [SUIT](https://datatracker.ietf.org/wg/suit/about/) compliance), using formally verified modules using [hax](https://hacspec.org/blog/posts/hax-v0-1/).


![Architecture](./doc/RIOT-rs-arch-diagram1.svg)


## Supported hardware

The following list of hardware is currently supported
 - [Nordic nRF52840 DK](https://www.nordicsemi.com/Products/Development-hardware/nRF52840-DK)
 - [Raspberry Pi Pico](https://www.raspberrypi.com/products/raspberry-pi-pico/)
 - more to come soon.

## Status

**This is currently work-in-progress. Expect missing functionalities and frequent changes!** 
If you are not so adventurous, but nevertheless looking for a way 
to run your Rust module on a microcontroller, you could try to 
glue it directly on top of the [embassy HAL](https://github.com/embassy-rs/embassy), 
or instead, run your module in a [riot-wrappers](https://gitlab.com/etonomy/riot-wrappers).

## Quickstart

Assuming you have a Nordic nrf52840dk connected to your PC, the following guidelines
provides instructions for flashing and running the [`hello-world`
example](https://github.com/future-proof-iot/RIOT-rs/tree/main/examples/hello-world):

### Prerequisites

1.install needed system dependencies. On Ubuntu, the following is sufficient:

        apt install build-essential curl git python3 pkg-config \
                   libssl-dev llvm-dev cmake libclang-dev gcc-arm-none-eabi \
                   clang libnewlib-nano-arm-none-eabi unzip lld ninja-build

1. install [rustup](https://rustup.rs/)

1. install [laze](https://github.com/kaspar030/laze): `cargo install laze`

1. install [probe-rs](https://github.com/probe-rs/probe-rs): `cargo install probe-rs --features cli`
   (2023-10-17: if that fails, try from git: `cargo install --git https://github.com/probe-rs/probe-rs --features cli probe-rs`)

1. clone this repository and cd into it

1. install rust targets: `laze build install-toolchain`

### Run the example

1. Compile, flash and the hello-world example using `probe-rs run`

        laze -C examples/hello-world build -b nrf52840dk -s probe-rs-run run

![Example](./doc/hello-world_render.svg)

<details>
<summary> (might fail if the flash is locked, click here for unlocking instructions) </summary>
This might fail due to a locked chip, e.g., on most nrf52840dk boards that are fresh from the factory.
In that case, the above command throws an error that ends with something like this:

```
An operation could not be performed because it lacked the permission to do so: erase_all
```

The chip can be unlocked using this command:

    laze -C examples/hello-world build -b nrf52840dk flash-erase-all
</details>

## More information

Please look [at the build system documentation](doc/build_system.md) for more usage
information.

## Minimum Supported Rust Version (MSRV)

RIOT-rs makes heavy use of Rust unstable features. For the time being, it is
recommended to use a current nightly.

## Coding Conventions

Please see the chapter on
[coding conventions](https://future-proof-iot.github.io/RIOT-rs/dev/coding-conventions.html)
in the documentation.

## Copyright & License

RIOT-rs is licensed under either of

- Apache License, Version 2.0 (LICENSE-APACHE or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license (LICENSE-MIT or http://opensource.org/licenses/MIT)

at your option.

~~RIOT-rs links with many components of [RIOT OS](https://github.com/RIOT-OS/RIOT),
which is licenced under the terms of LGPLv2.1.~~

Copyright (C) 2020-2023 Freie Universität Berlin, Inria, Kaspar Schleiser

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall
be dual licensed as above, without any additional terms or conditions.

[build-badge]: https://github.com/future-proof-iot/RIOT-rs/actions/workflows/main.yml/badge.svg
[build-info]: https://github.com/future-proof-iot/RIOT-rs/actions/workflows/main.yml
[matrix-badge]: https://img.shields.io/badge/chat-Matrix-brightgreen.svg
[matrix-link]: https://matrix.to/#/#RIOT-rs:matrix.org
[doc-badge]: https://img.shields.io/badge/Documentation-%F0%9F%93%94-blue
[documentation-mdbook]: https://future-proof-iot.github.io/RIOT-rs/dev/
