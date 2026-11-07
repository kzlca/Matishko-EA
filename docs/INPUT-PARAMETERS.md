# Input parameters — v1.25

Both MT4 and MT5 v1.25 expose the same 15 inputs in the same order, with the same names, defaults, and trade-direction values. Broker symbol specifications, volume limits, and minimum volume steps affect actual order execution.

> **Default-setting notice:** The shipped defaults are reference starting values only. They have not been established through systematic backtesting, parameter optimization, or broker-specific forward testing. They are not a recommended or optimized preset. Select and validate settings for your symbol, timeframe, broker, account conditions, and risk tolerance using the [Strategy Testing and Optimization Guide](STRATEGY-TESTING.md).

| Section | Input | Default | Description |
|---|---|---:|---|
| Martingale & grid | `InitialLots` | `0.01` | Starting position volume. |
| Martingale & grid | `LotSizeMultiplier` | `1.6` | Volume multiplier for each additional basket position. |
| Martingale & grid | `GridStepPips` | `5.0` | Base grid spacing; the legacy calculation uses broker points. |
| Martingale & grid | `DistanceMultiplier` | `1.5` | Expands the spacing of later grid entries. |
| Martingale & grid | `MultiplierStartOrder` | `2` | Position count from which expanded grid spacing begins. |
| Martingale & grid | `MaxOpenOrders` | `10` | Maximum simultaneous positions/orders for the EA on the symbol. |
| Basket exit | `TakeProfitPips` | `150.0` | Basket target distance from the volume-weighted average entry; legacy formula uses broker points. |
| Basket exit | `UseTrailingStop` | `false` | Enables per-position trailing stops. |
| Basket exit | `TrailingStartPips` | `35.0` | Trailing activation threshold; legacy formula uses broker points. |
| Basket exit | `TrailingStopPips` | `49.0` | Trailing-stop distance, adjusted for symbol quote precision. |
| Equity protection | `UseSafeEquity` | `false` | Enables the EA's equity drawdown protection. |
| Equity protection | `SafeEquityRiskPercent` | `10.0` | Floating-loss threshold as a percentage of recorded peak account equity. |
| Execution & identification | `Slippage` | `3.0` | Maximum execution deviation input, adjusted for quote precision. |
| Execution & identification | `MagicNumber` | `999` | Identifier used to distinguish the EA's trades from other trades. |
| Trade direction | `TradeDirection` | `Both` (`0`) | Controls the direction of new initial entries and new grid additions: `Both` (`0`), `Buy Only` (`1`), or `Sell Only` (`2`). Existing positions remain managed. |

## Direction selection

- **Both (`0`):** new BUY and SELL entries are permitted according to the strategy's existing entry conditions.
- **Buy Only (`1`):** new BUY initial entries and BUY grid additions are permitted; new SELL initial entries and SELL grid additions are blocked.
- **Sell Only (`2`):** new SELL initial entries and SELL grid additions are permitted; new BUY initial entries and BUY grid additions are blocked.

The selected input initializes the on-chart direction control. The chart button can still change the runtime mode during a visual test or live chart session. Reinitializing the EA restores the input selection. Changing the permitted direction does not force-close existing positions; their normal basket management continues.

### MT4 Strategy Tester

The MT4 tester's **Common → Positions** restriction is independent of the EA input and does not change the dashboard label. Set it to **Long & Short** when using `TradeDirection` as the controlling setting. Otherwise, the platform-level restriction can further filter executions.

### MT5 account mode

The MT5 build requires a **hedging account**. Netting accounts are not supported by the multi-position grid strategy.

## Legacy distance units

Despite their names, `GridStepPips`, `TakeProfitPips`, and `TrailingStartPips` use broker points in the legacy formulas, not conventional Forex pips. Review the symbol's digits and point size before changing these values.
