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

## Platform compatibility

- **MetaTrader 4:** use the `.ex4` release asset.
- **MetaTrader 5:** use the `.ex5` release asset and an MT5 **hedging account**. Netting accounts are not supported by this multi-position strategy.

## Installation

1. Download the binary for your MetaTrader platform from the v1.24 release.
2. In MetaTrader, choose **File → Open Data Folder**.
3. For MT4, copy the `.ex4` file into `MQL4/Experts/`. For MT5, copy the `.ex5` file into `MQL5/Experts/` (or a subfolder).
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
