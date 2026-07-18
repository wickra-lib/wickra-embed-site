---
layout: home
title: wickra-embed — no_std indicators for bare metal
titleTemplate: false

hero:
  name: "wickra-embed"
  text: "Indicators where there is no OS."
  tagline: "Allocation-free, #![no_std] streaming indicators for bare-metal and HFT — O(1) with bounded latency, fixed-capacity buffers, no heap. Byte-for-byte identical to wickra-core."
  image:
    src: /wickra-mark.svg
    alt: wickra-embed
  actions:
    - theme: brand
      text: View on GitHub
      link: https://github.com/wickra-lib/wickra-embed
    - theme: alt
      text: Latency & memory
      link: https://github.com/wickra-lib/wickra-embed/blob/main/BENCHMARKS.md
    - theme: alt
      text: API
      link: /api/rust

features:
  - icon: 🔩
    title: No OS, no heap
    details: wickra-embed runs the Wickra indicator math where there is no operating system and no allocator — microcontrollers, FPGA soft-cores, HFT co-processors. Fixed-capacity buffers, zero allocation, ever.
  - icon: ⏱️
    title: O(1), bounded latency
    details: "Every update is O(1) with a bounded worst-case latency — the property a hard-real-time or HFT hot loop needs. No amortized surprises, no reallocation stalls."
  - icon: 🎯
    title: Byte-for-byte identical
    details: "Each indicator produces the byte-for-byte identical value the std wickra-core produces on a server — verified by a parity test suite, so the bare-metal path and the cloud path never disagree."
  - icon: 📦
    title: Five verified indicators
    details: "The v0.1 line ships 5 indicators — five verified no-alloc primitives — Sma, Ema, Rsi, Atr and Roc — the distillation of the wickra-core math down to what runs with no heap."
  - icon: 🦀
    title: Rust core + C ABI
    details: "A Rust #![no_std] crate (embed-core) plus a C ABI header and static library, so it drops into a Rust firmware project or any C / C++ toolchain for an MCU or FPGA soft-core."
  - icon: 🧪
    title: Deterministic, proven
    details: The bare-metal output matches wickra-core exactly, pinned by a parity corpus in CI. No wall-clock, no platform floats drift — the same input yields the same value everywhere.
---

<script setup>
const installTabs = [
  { label: 'Rust', lang: 'bash', code: 'cargo add embed-core' },
  { label: 'C',    lang: 'bash', code: '# prebuilt wickra_embed.h + static library per target:\n# github.com/wickra-lib/wickra-embed/releases' },
]

const rustCode = `#![no_std]
use embed_core::{Sma, Indicator};

let mut sma = Sma::new(20).expect("valid period");

for &price in &prices {
    // O(1), allocation-free; None during warmup.
    if let Some(avg) = sma.update(price) {
        // use avg — byte-identical to wickra-core
    }
}`

const cCode = `#include "wickra_embed.h"

WickraSma *sma = wickra_sma_new(20);
double out;
for (size_t i = 0; i < n; i++) {
    /* status == 0 once warm; writes the average into out */
    if (wickra_sma_update(sma, prices[i], &out) == 0) {
        /* use out */
    }
}
wickra_sma_free(sma);`

const snippetTabs = [
  { label: 'Rust', lang: 'rust', code: rustCode },
  { label: 'C',    lang: 'c',    code: cCode },
]
</script>

## Allocation-free, on bare metal

Every indicator is a fixed-size state machine. There is no `Vec`, no `Box`, no
allocator call — the whole thing lives on the stack or in a static, so it runs on
a microcontroller with a few KB of RAM, and its worst-case update latency is
bounded.

```rust
use embed_core::{Ema, Rsi, Indicator};

let mut ema = Ema::new(12).unwrap();
let mut rsi = Rsi::new(14).unwrap();

// Feed one tick; both update in O(1) with no allocation.
let e = ema.update(price);
let r = rsi.update(price);
```

## Install

A Rust `#![no_std]` crate, plus a C ABI header + static library for firmware
toolchains.

<InstallTabs :tabs="installTabs" />

## Use it from Rust or C

The same five indicators — `Sma`, `Ema`, `Rsi`, `Atr`, `Roc` — from a Rust
firmware project or a C / C++ toolchain.

<InstallTabs :tabs="snippetTabs" />

## Built on the Wickra core

wickra-embed is the no-alloc, bare-metal distillation of
[`wickra-core`](https://github.com/wickra-lib/wickra). Each indicator is
byte-for-byte identical to its std counterpart, so a signal computed on a
microcontroller matches the one a server or a live chart would compute.

> wickra-embed is a software library, not a trading system, and comes with no
> warranty — use at your own risk.
