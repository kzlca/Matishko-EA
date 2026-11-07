# Matishko EA

**Martingale-based grid Expert Advisor for MetaTrader 4 and MetaTrader 5**

Matishko EA manages trades as a basket using configurable grid entries, progressive position sizing, and a volume-weighted average take-profit target. It is designed to complement an independent directional strategy; it does not generate or validate market signals.

[![Matishko EA dashboard](screenshots/Screenshot_20261106_004830.png)](screenshots/Screenshot_20261106_004830.png)

## Downloads

**Latest release: v1.25**

| Platform | Download |
|---|---|
| MetaTrader 4 | [Matishko_EA_v1.25.ex4](https://github.com/kzlca/Matishko-EA/releases/download/v1.25/Matishko_EA_v1.25.ex4) |
| MetaTrader 5 | [Matishko_EA_v1.25.ex5](https://github.com/kzlca/Matishko-EA/releases/download/v1.25/Matishko_EA_v1.25.ex5) |

See the [v1.25 release manifest](docs/RELEASE-v1.25.md) for file sizes and SHA-256 checksums.

## Trade-direction settings

The `TradeDirection` input has the same values and default on both platforms:

| Setting | Value | Behavior for new entries |
|---|---:|---|
| `Both` | `0` | BUY and SELL entries may be opened when the strategy conditions are met. |
| `Buy Only` | `1` | Only BUY initial entries and BUY grid additions are permitted. |
| `Sell Only` | `2` | Only SELL initial entries and SELL grid additions are permitted. |

The selected input initializes the on-chart direction control. Changing the panel control during a visual test or live session changes the runtime direction; reinitializing the EA restores the input selection. Changing direction does **not** force-close or stop managing positions that are already open.

### MT4 Strategy Tester note

The MT4 Strategy Tester has its own **Common → Positions** restriction. It operates independently of the EA input and is not reflected in the EA dashboard. When you want `TradeDirection` to be the single direction selector, set the tester restriction to **Long & Short**, then select `TradeDirection` under **Inputs**. Otherwise, the tester can further restrict executions while the panel still shows the EA's selected mode.

## Recommended operating approach

Matishko EA is intended to manage a basket after a trader or an independent strategy identifies a possible directional opportunity.

- **Bullish directional signal:** select `Buy Only`.
- **Bearish directional signal:** select `Sell Only`.
- **No directional restriction:** select `Both`.

If the initial entry moves against the basket and price later reverses sufficiently, the changed volume-weighted average entry may allow the basket to reach its take-profit target. This is a possible recovery mechanism, not a guarantee. Continued adverse movement can increase exposure and floating losses significantly.

## Strategy Tester and parameter selection

**The shipped input defaults are reference starting values, not backtested or optimized recommendations.** They have not been established through a systematic historical backtest or broker-specific forward test. Do not assume they are suitable for your symbol, timeframe, broker, account size, or risk tolerance.

Use the MetaTrader Strategy Tester to evaluate and optimize your own settings. Test realistic historical data, spreads, commissions, swaps, leverage, and margin conditions. Use separate historical periods for optimization and out-of-sample validation, then forward-test the selected configuration on a demo account.

For a Martingale grid, evaluate maximum equity drawdown, margin usage, largest basket exposure, and time spent in floating loss—not just net profit or profit factor.

Read the [Strategy Testing and Optimization Guide](docs/STRATEGY-TESTING.md) and [Input parameters](docs/INPUT-PARAMETERS.md) before selecting settings.

### Instruments worth evaluating

Higher-volatility instruments such as **XAUUSD (gold)** may be worth including in your tests. This is a test candidate, not a claim that the EA performs better on gold. Gold is commonly offered as a metal CFD rather than a Forex currency pair, and its contract size, tick size/value, spread, commission, swap, trading hours, and margin requirements are broker-specific.

A higher nominal quote price does not make an instrument more suitable for a Martingale grid. Larger or faster price swings can trigger more grid entries and increase floating losses and margin usage more quickly. Test the exact broker symbol and its contract specifications, optimize it separately, and require out-of-sample validation and demo forward testing.

## Platform compatibility

- **MetaTrader 4:** use the `.ex4` release asset.
- **MetaTrader 5:** use the `.ex5` release asset and an MT5 **hedging account**. Netting accounts are not supported by this multi-position strategy.

## Installation

1. Download the binary for your platform from the [v1.25 release](https://github.com/kzlca/Matishko-EA/releases/tag/v1.25).
2. In MetaTrader, choose **File → Open Data Folder**.
3. For MT4, copy the `.ex4` file into `MQL4/Experts/`. For MT5, copy the `.ex5` file into `MQL5/Experts/` (or a subfolder).
4. Refresh the Expert Advisors list in Navigator or restart the terminal.
5. Attach Matishko EA to a chart and review all inputs, including `TradeDirection`.
6. Test the exact build on a demo account before considering live use.

See the [installation guide](docs/INSTALLATION.md).

## Expiry

The v1.25 builds have an operational expiry date of **31 December 2028 (23:59:59 broker-server time)**. New entries are disabled after expiry; management of existing positions remains active.

## Risk information

Matishko EA uses a Martingale-based grid approach. Position volume can grow geometrically as additional entries are added. A prolonged move against the basket can produce substantial floating losses, increasing margin usage and potentially causing stop-out or loss of the trading balance.

Trailing stops, basket take-profit, maximum-position limits, and equity protection do not eliminate market, liquidity, execution, or platform risk. No profitability or capital-protection guarantee is made. Read [Risk and limitations](docs/RISK-AND-LIMITATIONS.md) before use.

## Proprietary software

Matishko EA is proprietary software. The `.mq4` and `.mq5` source files are not included in this public repository or release assets. Only the compiled platform binaries and user documentation are distributed.

## Support

For reproducible installation problems or software defects, open a [GitHub Issue](https://github.com/kzlca/Matishko-EA/issues). Do not include account numbers, passwords, investor credentials, or other sensitive information.
