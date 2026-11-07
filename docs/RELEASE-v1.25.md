# Matishko EA v1.25 — release manifest

| Field | Value |
|---|---|
| Product | Matishko EA |
| Version | 1.25 |
| Platforms | MetaTrader 4 and MetaTrader 5 |
| MT4 asset | [Matishko_EA_v1.25.ex4](https://github.com/kzlca/Matishko-EA/releases/download/v1.25/Matishko_EA_v1.25.ex4) |
| MT5 asset | [Matishko_EA_v1.25.ex5](https://github.com/kzlca/Matishko-EA/releases/download/v1.25/Matishko_EA_v1.25.ex5) |
| MT4 size | 76360 bytes |
| MT5 size | 155268 bytes |
| MT4 SHA-256 | `d9c6b8fec8c2cf58b5ffa94c22cb714c8f3b0d8e573800527f43f4efc4a2f3ad` |
| MT5 SHA-256 | `fba5bfa250bed01c4b3292b3174ddaf71f6506b5e0dcd684f4803afeb7d46392` |
| MT5 account mode | Hedging |
| Expiry | 31 December 2028, 23:59:59 broker-server time |

## Verify the downloads

Download both assets, then run:

```bash
sha256sum Matishko_EA_v1.25.ex4 Matishko_EA_v1.25.ex5
```

The output must match the hashes above. A checksum confirms file integrity; it does not certify trading performance.

## v1.25 changes

- Shared `TradeDirection` input on both platforms (`Both`, `Buy Only`, `Sell Only`).
- Direction is applied to initial entries and grid additions, and initializes the chart direction control.
- Existing positions continue to be managed after changing direction.
- MT4 Strategy Tester Common → Positions must be set to Long & Short when the EA input is intended to be the sole direction restriction.
- Expiry information is displayed consistently in both dashboards; expiry enforcement remains on broker-server time.
- MT5 hedging-account compatibility and fixed v1.24 expiry validation are retained.

The Martingale sizing, grid spacing, basket TP, trailing-stop, and equity-protection strategy formulas are not intentionally changed by this release's direction-input update.
