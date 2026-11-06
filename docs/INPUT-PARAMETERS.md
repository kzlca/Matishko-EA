# Input parameters

The v1.24 MT4 and MT5 builds use the following 14 trading inputs. Values are subject to broker symbol specifications, volume limits, and minimum volume steps.

> **Default-setting notice:** The shipped defaults are reference starting values only. They have not been established through systematic backtesting, parameter optimization, or broker-specific forward testing. They are not a recommended or optimized preset. Select and validate settings for your symbol, timeframe, broker, account conditions, and risk tolerance using the [Strategy Testing and Optimization Guide](STRATEGY-TESTING.md).

| Section | Input | Default | Description |
|---|---|---:|---|
| Martingale & grid | `InitialLots` | `0.01` | Starting position volume. |
| Martingale & grid | `LotSizeMultiplier` | `1.6` | Volume multiplier for each additional basket position. |
| Martingale & grid | `GridStepPips` | `5.0` | Base grid spacing. This legacy parameter is interpreted in broker points by the strategy formula. |
| Martingale & grid | `DistanceMultiplier` | `1.5` | Expands the spacing of later grid entries. |
| Martingale & grid | `MultiplierStartOrder` | `2` | Position count from which expanded grid spacing begins. |
| Martingale & grid | `MaxOpenOrders` | `10` | Maximum simultaneous positions for the EA on the symbol. |
| Basket exit | `TakeProfitPips` | `150.0` | Basket target distance from the volume-weighted average entry. The legacy formula uses broker points. |
| Basket exit | `UseTrailingStop` | `false` | Enables per-position trailing stops. |
| Basket exit | `TrailingStartPips` | `35.0` | Price movement required to activate trailing, using the legacy point-based formula. |
| Basket exit | `TrailingStopPips` | `49.0` | Trailing-stop distance; adjusted for symbol quote precision. |
| Equity protection | `UseSafeEquity` | `false` | Enables the EA's equity drawdown protection. |
| Equity protection | `SafeEquityRiskPercent` | `10.0` | Floating-loss threshold as a percentage of recorded peak account equity. |
| Execution & identification | `Slippage` | `3.0` | Maximum execution deviation input, adjusted for quote precision. |
| Execution & identification | `MagicNumber` | `999` | Identifier used to distinguish the EA's trades from other trades. |

## Position sizing

With the default sizing mode, the next position volume is based on the initial lot size multiplied by the lot multiplier raised to the number of currently open positions in the EA's basket. Broker volume minimums, maximums, and step sizes affect the final submitted volume.

## Legacy distance units

Some distance inputs retain their historical names for compatibility. In particular, `GridStepPips`, `TakeProfitPips`, and `TrailingStartPips` use broker points in the legacy calculations, not conventional Forex pips. Review the symbol's digits and point size before changing these values.
