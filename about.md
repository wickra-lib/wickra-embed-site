# About wickra-embed

wickra-embed runs the Wickra indicator math where there is no operating system and
no heap: microcontrollers, FPGA soft-cores, HFT co-processors. Every update is O(1)
with a bounded worst-case latency, uses fixed-capacity buffers (no allocation), and
produces the **byte-for-byte identical** value the std `wickra-core` produces on a
server — verified by a parity test suite.

## What makes it different

- **No OS, no heap.** A `#![no_std]`, allocation-free crate. Every indicator is a
  fixed-size state machine that lives on the stack or in a static — it runs on a
  microcontroller with a few KB of RAM.
- **O(1), bounded latency.** Every update is constant-time with a bounded worst
  case, the property a hard-real-time or HFT hot loop needs.
- **Byte-for-byte identical.** Each indicator matches its std `wickra-core`
  counterpart exactly, pinned by a parity corpus in CI, so the bare-metal path and
  the cloud path never disagree.
- **Rust core + C ABI.** A Rust `#![no_std]` crate (`embed-core`) plus a C ABI
  header and static library, so it drops into a Rust firmware project or any C / C++
  toolchain.

## What it ships

The v0.1 line ships five verified no-alloc indicators — `Sma`, `Ema`, `Rsi`, `Atr`
and `Roc` — the distillation of the `wickra-core` math down to what runs with no
heap. More follow as they are made allocation-free and parity-verified.

## Why it exists

The full Wickra library assumes `std` and an allocator. wickra-embed is the
bare-metal distillation of that same math, so a strategy can compute the exact same
signals on a microcontroller at the edge as it does in the cloud — no reimplementation,
no drift.

## Open source

Released under the **MIT OR Apache-2.0** license — permissive, OSI-approved, free
for any use including commercial. Source, issues and releases on
[GitHub](https://github.com/wickra-lib/wickra-embed).

## Disclaimer

wickra-embed is a software library, **not** a trading system, and is provided
**as-is with no warranty**. It computes indicator values on-device; it does not give
financial advice. Use it at your own risk.
