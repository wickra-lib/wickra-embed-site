# Rust

The `#![no_std]` crate. Every indicator is a fixed-size, allocation-free state
machine you update one tick at a time.

```bash
cargo add embed-core
```

```rust
#![no_std]
use embed_core::{Sma, Ema, Rsi, Indicator};

let mut sma = Sma::new(20).expect("valid period");
let mut ema = Ema::new(12).expect("valid period");
let mut rsi = Rsi::new(14).expect("valid period");

for &price in &prices {
    // Each update is O(1) with no allocation; None during warmup.
    let s = sma.update(price);
    let e = ema.update(price);
    let r = rsi.update(price);
    let _ = (s, e, r); // byte-identical to wickra-core
}
```

`Sma`, `Ema`, `Rsi`, `Atr` and `Roc` implement the `Indicator` trait. `Atr` takes
a `Candle` (`update(candle)`); the others take an `f64`.

## More

- [crates.io/crates/embed-core](https://crates.io/crates/embed-core) - [docs.rs](https://docs.rs/embed-core)
- [Source & examples](https://github.com/wickra-lib/wickra-embed/tree/main/examples)
- [Latency & memory](https://github.com/wickra-lib/wickra-embed/blob/main/BENCHMARKS.md)
