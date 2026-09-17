---
title: Indicator demo
description: The Wickra indicator core that Wickra Embed is built on, running in your browser — powered by the wickra-wasm bundle from npm.
---

# The core, live

Every line below is computed by **`wickra-wasm`** — the indicator core
Wickra Embed is built on — running inside your browser via WebAssembly:
no server, no API call, no install. The same Rust kernel that computes every
indicator Wickra Embed reads is producing every value you see.

> **▶ Want real market data?** This page runs every indicator over a *generated*
> price series. For **all 514 indicators over a live Binance feed** — candles,
> trades and order-book depth, still 100% in your browser — head to
> **[live.wickra.org](https://live.wickra.org)**.

<WasmDemo />

The picker carries the best-known indicators from each family — moving
averages, oscillators, bands, trailing stops, pivots, Ichimoku,
candlestick / chart / harmonic patterns, Fibonacci and more. Whatever
its shape — a single line, a set of bands, a sub-pane oscillator, or
pattern markers — it is the same Rust kernel computing it tick by tick.

## What's happening

1. A pseudo-random walk generates a synthetic **OHLCV candle** stream
   (start = 100), shaped live by the **Volatility** and **Trend**
   sliders.
2. The wickra-wasm bundle is fetched once (~80 KB gzipped) and the
   indicator class you pick is instantiated.
3. Every candle is fed into the indicator through its native input —
   close, full OHLC, high/low, or close + volume — in a single O(1)
   state-machine step. Single values draw a line, multi-output
   indicators draw a band per field, and pattern detectors drop
   markers on the chart.
4. Hitting **▶ Stream** advances the feed in real time; the **Stream
   speed** slider (1–10) sets how many candles per second arrive, and
   **⏸ Pause** freezes it without losing state.

## Try this

- Switch to **Bollinger Bands**, then crank **Volatility** up — the
  band envelope visibly widens as the synthetic asset gets noisier.
- Pick **Engulfing** (Candlestick Patterns) and toggle **Candles** on —
  arrows mark each detected pattern against the real candle bodies.
- Pull **Trend** toward *bull* and watch **SuperTrend** flip its stop
  line and ride the move.
- Raise **Stream speed** to 10 — the chart fills fast while every tick
  is still a real O(1) WASM update, not a replay.

## Wiring this into your own project

The chart on this page is ~450 lines of Vue + lightweight-charts; the
indicator math is one constructor call and `update()` per tick.

```javascript
import init, { RSI } from 'wickra-wasm'

await init()                    // fetches the .wasm bundle
const rsi = new RSI(14)

for (const price of liveFeed) {
  const v = rsi.update(price)   // O(1) — returns null during warmup
  if (v !== null && v > 70) console.log('overbought', v)
}
```

The same shape works in Node and Python — the core's own
[API pages](https://wickra.org/api/wasm) carry each binding's idiomatic snippet;
the [API pages here](/api/rust) are Wickra Embed's.
