# C / C++

The C ABI. A single `wickra_embed.h` header + a static library link the same
allocation-free indicators from C, C++, or any firmware toolchain.

```bash
# prebuilt wickra_embed.h + static library per target:
# github.com/wickra-lib/wickra-embed/releases
```

```c
#include "wickra_embed.h"

WickraSma *sma = wickra_sma_new(20);
double out;

for (size_t i = 0; i < n; i++) {
    /* Returns 0 once warm and writes the average into out. */
    if (wickra_sma_update(sma, prices[i], &out) == 0) {
        /* use out — byte-identical to wickra-core */
    }
}

wickra_sma_free(sma);
```

Each indicator (`Sma`, `Ema`, `Rsi`, `Atr`, `Roc`) exposes `wickra_<name>_new`,
`wickra_<name>_update` and `wickra_<name>_free`. `wickra_embed_version()` returns
the library version. No allocation happens inside `update`.

## More

- [C ABI header + examples](https://github.com/wickra-lib/wickra-embed/tree/main/examples)
- [Releases (prebuilt header + library)](https://github.com/wickra-lib/wickra-embed/releases)
