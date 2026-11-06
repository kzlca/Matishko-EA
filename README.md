# Matishko EA

**Martingale-based grid Expert Advisor for MetaTrader 4 and MetaTrader 5**

Matishko EA is an automated trading system built around progressive grid entries and Martingale position sizing. It manages trades as a basket using configurable entry spacing, volume scaling, and a weighted-average take-profit target.

[![Matishko EA dashboard](screenshots/Screenshot_20261106_004830.png)](screenshots/Screenshot_20261106_004830.png)

## Downloads

**Latest release: v1.24**

| Platform | Download |
|---|---|
| MetaTrader 4 | [Matishko_EA_v1.24.ex4](https://github.com/kzlca/Matishko-EA/releases/download/v1.24/Matishko_EA_v1.24.ex4) |
| MetaTrader 5 | [Matishko_EA_v1.24.ex5](https://github.com/kzlca/Matishko-EA/releases/download/v1.24/Matishko_EA_v1.24.ex5) |

See the [release manifest](docs/RELEASE-v1.24.md) for file checksums and build information.

## Strategy features

- Martingale position sizing with configurable initial volume and lot multiplier.
- Grid entries with configurable base spacing and distance expansion.
- Basket take-profit based on the volume-weighted average entry price.
- Optional per-position trailing stops.
- Optional equity drawdown protection.
- Maximum open-position limit and configurable trade identification (Magic Number).
- On-chart dashboard and controls for monitoring the active basket and direction mode.

## Recommended operating approach

Matishko EA is designed to complement an independent trading strategy rather than replace market analysis. Consider activating it after your own trading method identifies a potential directional opportunity.

- **Bullish signal:** select BUY-only mode.
- **Bearish signal:** select SELL-only mode.
- **Basket management:** allow the EA to manage grid entries, position sizing, and the basket exit according to the configured inputs.

If the initial entry moves against the basket and price later reverses sufficiently, the changed volume-weighted average entry may allow the basket to reach its take-profit target. This is a possible recovery mechanism, not a guarantee. Continued adverse movement can increase exposure and floating losses significantly.

Matishko EA does not generate or validate an external directional signal. Signal identification remains the trader's responsibility.

## Strategy Tester and parameter selection

**The shipped input defaults are reference starting values, not backtested or optimized recommendations.** They have not been established through a systematic historical backtest or broker-specific forward test. Do not assume they are suitable for your symbol, timeframe, broker, account size, or risk tolerance.

Use the MetaTrader Strategy Tester to evaluate and optimize your own settings. Test realistic historical data, spreads, commissions, swaps, leverage, and margin conditions. Use separate historical periods for optimization and out-of-sample validation, then forward-test the selected configuration on a demo account.

For a Martingale grid, evaluate maximum equity drawdown, margin usage, largest basket exposure, and time spent in floating loss—not just net profit or profit factor.

Read the [Strategy Testing and Optimization Guide](docs/STRATEGY-TESTING.md) before selecting settings.

### Instruments worth evaluating

Higher-volatility instruments such as **XAUUSD (gold)** may be worth including in your tests. This is a suggestion to evaluate them, not a claim that Matishko EA performs better on gold. Gold is commonly offered by brokers as a metal CFD rather than a Forex currency pair, and its contract size, tick size, tick value, spread, commission, swap, trading hours, and margin requirements can differ substantially from Forex symbols.

The instrument's nominal price being higher does not make it more suitable for a Martingale grid. Larger or faster price swings can create more grid entries and can also increase floating losses and margin usage more quickly. Test the exact broker symbol (including any suffix), use its actual contract specifications, and optimize its settings separately. Do not transfer Forex presets to gold without testing. Require out-of-sample validation and demo forward testing before considering live use.

## Platform compatibility

- **MetaTrader 4:** use the .ex4 release asset.
- **MetaTrader 5:** use the .ex5 release asset and an MT5 **hedging account**. Netting accounts are not supported by this multi-position strategy.

## Installation

1. Download the binary for your MetaTrader platform from the v1.24 release.
2. In MetaTrader, choose **File → Open Data Folder**.
3. For MT4, copy the .ex4 file into MQL4/Experts/. For MT5, copy the .ex5 file into MQL5/Experts/ (or a subfolder).
4. Refresh the Expert Advisors list in Navigator or restart the terminal.
5. Attach Matishko EA to a chart, review all inputs, and test on a demo account.

See the [installation guide](docs/INSTALLATION.md) for platform-specific steps.

## Risk information

Matishko EA uses a Martingale-based grid approach. Position volume can grow geometrically as additional entries are added. A prolonged move against the basket can produce substantial floating losses, increasing margin usage and potentially causing stop-out or loss of the trading balance.

Trailing stops, basket take-profit, maximum-position limits, and equity protection do not eliminate market, liquidity, execution, or platform risk. No profitability or capital-protection guarantee is made. Read [Risk and limitations](docs/RISK-AND-LIMITATIONS.md) before use.

## Expiry

The v1.24 build has an operational expiry date of **31 December 2028 (23:59:59 broker-server time)**. New entries are disabled after expiry; management of existing positions remains active.

## Support

For reproducible installation problems or software defects, open a [GitHub Issue](https://github.com/kzlca/Matishko-EA/issues). Do not include account numbers, passwords, investor credentials, or other sensitive information.
