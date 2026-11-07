# Changelog

## v1.25 — MetaTrader 4 and MetaTrader 5

- Added a shared `TradeDirection` input to both platforms: `Both`, `Buy Only`, and `Sell Only`.
- Applied the selected direction to new initial entries and additional grid entries.
- Initialized each chart's direction control from the selected input; existing positions continue to be managed normally after a direction change.
- Aligned the MT4 Strategy Tester workflow with MT5 by making direction a visible EA input; the MT4 tester's independent Common → Positions filter remains available and should be set to Long & Short when using the EA input as the sole selector.
- Made expiry information visible consistently in both platform dashboards while retaining the existing expiry timestamp and broker-server-time validation.
- Retained the MT5 hedging-account requirement and the recovered v1.24 expiry validation.
- No intentional change to the existing Martingale sizing, grid spacing, basket take-profit, trailing-stop, or equity-protection strategy formulas.

## v1.24 — MetaTrader 4 and MetaTrader 5

- Published coordinated MT4 and MT5 compiled releases.
- Unified the Martingale sizing, progressive grid, and basket-exit behavior across the two platform implementations, subject to platform-specific execution differences.
- Organized the trading inputs into clear sections with matching names and defaults.
- Updated the on-chart dashboard and operational controls.
- Replaced legacy account-specific activation with an expiry date of 31 December 2028.
