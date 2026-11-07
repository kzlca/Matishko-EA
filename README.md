# Matishko EA

**Martingale-based grid Expert Advisor for MetaTrader 4 and MetaTrader 5**

Matishko EA manages trades as a basket using configurable grid entries, progressive position sizing, and a volume-weighted average take-profit target. It is designed to complement an independent directional strategy;

[![Matishko EA dashboard](screenshots/Screenshot_20261106_004830.png)](screenshots/Screenshot_20261106_004830.png)

## Downloads

**Latest release: v1.25**

| Platform | Download |
|---|---|
| MetaTrader 4 | [Matishko_EA_v1.25.ex4](https://github.com/kzlca/Matishko-EA/releases/download/v1.25/Matishko_EA_v1.25.ex4) |
| MetaTrader 5 | [Matishko_EA_v1.25.ex5](https://github.com/kzlca/Matishko-EA/releases/download/v1.25/Matishko_EA_v1.25.ex5) |

See the [v1.25 release manifest](docs/RELEASE-v1.25.md) for file sizes and SHA-256 checksums.

## Recommended operating approach

Matishko EA is intended to manage a basket after a trader or an independent strategy identifies a possible directional opportunity.

- **Bullish directional signal:** select `Buy Only`.
- **Bearish directional signal:** select `Sell Only`.
- **Ranging Market:** select `Both Mode`. 
- **No signal:** pause `EA`.

If the initial entry moves against the basket and price later reverses sufficiently, the changed volume-weighted average entry may allow the basket to reach its take-profit target. This is a possible recovery mechanism, not a guarantee. Continued adverse movement can increase exposure and floating losses significantly.

## Strategy Tester and parameter selection

**The shipped input defaults are reference starting values, not backtested or optimized recommendations.** They have not been established through a systematic historical backtest or broker-specific forward test. Do not assume they are suitable for your symbol, timeframe, broker, account size, or risk tolerance.

Use the MetaTrader Strategy Tester to evaluate and optimize your own settings. Test realistic historical data, spreads, commissions, swaps, leverage, and margin conditions. Use separate historical periods for optimization and out-of-sample validation, then forward-test the selected configuration on a demo account.

For a Martingale grid, evaluate maximum equity drawdown, margin usage, largest basket exposure, and time spent in floating loss—not just net profit or profit factor.

Read the [Strategy Testing and Optimization Guide](docs/STRATEGY-TESTING.md) and [Input parameters](docs/INPUT-PARAMETERS.md) before selecting settings.

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

## Risk information

Matishko EA uses a Martingale-based grid approach. Position volume can grow geometrically as additional entries are added. A prolonged move against the basket can produce substantial floating losses, increasing margin usage and potentially causing stop-out or loss of the trading balance.

Trailing stops, basket take-profit, maximum-position limits, and equity protection do not eliminate market, liquidity, execution, or platform risk. No profitability or capital-protection guarantee is made. Read [Risk and limitations](docs/RISK-AND-LIMITATIONS.md) before use.

## Proprietary software

Matishko EA is proprietary software. The `.mq4` and `.mq5` source files are not included in this public repository or release assets. Only the compiled platform binaries and user documentation are distributed.

## Support

For reproducible installation problems or software defects, open a [GitHub Issue](https://github.com/kzlca/Matishko-EA/issues). Do not include account numbers, passwords, investor credentials, or other sensitive information. 

## Complete User Guide

### 1. How Matishko EA works

Matishko EA is a Martingale-based grid Expert Advisor designed to manage a series of positions as a basket.

It is intended to complement an independent trading strategy. The trader identifies a possible market direction, selects the appropriate trading mode, and lets the EA manage position sizing, additional grid entries, and the basket exit.

Matishko EA does not independently analyze higher-timeframe trends or generate a sophisticated directional signal. Its initial entry logic uses the previous two completed candles on the chart timeframe.

- If the previous candle closed higher than the candle before it, the EA looks for a BUY entry.
- If the previous candle closed lower than the candle before it, the EA looks for a SELL entry.
- The selected trade direction determines which of these entries are permitted.

The EA evaluates its main trading actions once per new chart candle.

### 2. Trade direction

The `TradeDirection` input is available on both MT4 and MT5.

| Setting | Value | Behavior |
|---|---:|---|
| Both | 0 | Allows BUY and SELL initial entries |
| Buy Only | 1 | Allows BUY entries only |
| Sell Only | 2 | Allows SELL entries only |

**Recommended use:**

- When your independent strategy detects a bullish opportunity, select `Buy Only`.
- When your strategy detects a bearish opportunity, select `Sell Only`.
- Select `Both` when you want the EA to choose between the two directions using its built-in candle logic.

This setting affects both initial entries and subsequent grid entries.

Changing the direction does not close existing positions. Positions already opened continue to be managed, but the EA will not add further positions in a direction that is no longer permitted.

The chart direction button can also change the mode while the EA is running. It cycles through Both, Buy Only, and Sell Only. The selected input determines the starting mode whenever the EA is initialized again.

**MT4 Strategy Tester:** Set Common → Positions to `Long & Short` when you want the EA's `TradeDirection` input to control direction. The tester's own direction restriction can otherwise impose an additional restriction.

### 3. Martingale and grid settings

These settings control the number of positions, their volume, and how far apart additional positions are placed.

| Input | Default | What it controls |
|---|---:|---|
| `InitialLots` | 0.01 | Volume of the first position |
| `LotSizeMultiplier` | 1.6 | Volume multiplier for subsequent positions |
| `GridStepPips` | 5.0 | Base distance between grid entries |
| `DistanceMultiplier` | 1.5 | Expansion or contraction of later grid distances |
| `MultiplierStartOrder` | 2 | Position count at which distance expansion begins |
| `MaxOpenOrders` | 10 | Maximum simultaneous positions for this EA on the current symbol |

#### InitialLots

Defines the volume of the first position.

Increasing this value increases the exposure of the entire basket because subsequent position volumes are calculated from it.

For example, changing 0.01 to 0.02 doubles the starting position volume and, with the same multiplier, the raw volume of every subsequent position.

The final submitted volume is adjusted to match the broker's minimum lot, maximum lot, and volume step.

#### LotSizeMultiplier

Controls how quickly position volume grows.

The current strategy calculates the next position volume using:

`InitialLots × LotSizeMultiplier ^ OpenPositionCount`

With the default values of 0.01 and 1.6, the theoretical progression is:

| Position | Calculated volume |
|---|---:|
| 1 | 0.0100 |
| 2 | 0.0160 |
| 3 | 0.0256 |
| 4 | 0.0410 |
| 5 | 0.0655 |

Actual submitted volumes may differ because of broker volume-step rounding and limits.

A higher multiplier increases recovery potential if the market reverses, but also increases exposure and potential losses much faster.

A multiplier of 1 produces approximately equal position sizes. Values below 1 reduce subsequent position sizes.

#### GridStepPips

Defines the base price distance required before another position can be opened against the existing basket.

For a BUY basket, another BUY can be opened when the Ask price falls sufficiently below the most recent BUY entry.

For a SELL basket, another SELL can be opened when the Bid price rises sufficiently above the most recent SELL entry.

A smaller grid step means more frequent entries and faster accumulation of exposure.

A larger grid step means fewer entries and more distance between positions.

**Important:** Despite its legacy name, this input is measured in broker points in the current strategy calculation, not conventional Forex pips.

#### DistanceMultiplier

Controls how grid spacing changes as the basket grows.

With a value above 1, subsequent grid distances become progressively larger.

For example, with:

- GridStepPips = 5
- DistanceMultiplier = 1.5
- MultiplierStartOrder = 2

The theoretical grid distances are:

| Position being added | Required distance |
|---|---:|
| 2nd | 5 points |
| 3rd | 7.5 points |
| 4th | 11.25 points |
| 5th | 16.875 points |

Increasing this setting makes later grid entries more widely spaced.

A value between 0 and 1 contracts later distances. A value of 1 disables distance expansion.

#### MultiplierStartOrder

Determines when the distance multiplier begins affecting subsequent grid entries.

A higher value delays grid-distance expansion.

For example, changing it from 2 to 3 means the first three positions use the base spacing before the multiplier begins affecting the following entry.

This setting works together with `GridStepPips` and `DistanceMultiplier`.

#### MaxOpenOrders

Defines the maximum number of simultaneous market positions managed by the EA for the current symbol and Magic Number.

The default is 10.

Lower values limit the maximum number of grid entries. Higher values allow a basket to accumulate more positions, potentially increasing total exposure and margin requirements.

When the limit is reached, the EA stops adding further grid positions. Existing positions continue to be managed.

### 4. Basket exit settings

| Input | Default | What it controls |
|---|---:|---|
| `TakeProfitPips` | 150.0 | Basket take-profit distance |
| `UseTrailingStop` | false | Enables or disables per-position trailing stops |
| `TrailingStartPips` | 35.0 | Price movement required to activate trailing |
| `TrailingStopPips` | 49.0 | Trailing-stop distance |

#### TakeProfitPips

Defines the basket take-profit distance from the volume-weighted average entry price.

The EA calculates the average entry using the open position volumes, then calculates a common basket target.

For BUY baskets, the target is above the average entry.

For SELL baskets, the target is below the average entry.

When additional grid positions are opened, the average entry is recalculated and the target is updated.

A smaller target requires less favorable movement for the basket to reach its exit.

A larger target requires more favorable movement and may keep the basket open longer.

**Important:** This legacy input also uses broker points, not conventional Forex pips.

#### UseTrailingStop

Enables or disables trailing-stop management.

When enabled, the EA manages trailing stops individually for each position.

A trailing stop is only activated after the position has moved sufficiently into profit. The stop then follows the favorable price movement and is not moved backward to a worse level.

Trailing stops can help protect gains, but they do not guarantee execution at the requested price.

#### TrailingStartPips

Defines how much favorable price movement is required before trailing begins.

A lower value activates trailing earlier.

A higher value waits for a larger favorable movement before activating.

**Important:** In the current implementation, this threshold is compared using broker points, despite the legacy input name.

#### TrailingStopPips

Defines the distance between the current market price and the trailing stop.

A smaller distance creates a tighter trailing stop. It may protect gains sooner but can also cause positions to close during ordinary market fluctuations.

A larger distance allows more room for price movement but may give back more floating profit.

Unlike `TrailingStartPips`, this input is converted from conventional pips to broker points for 3- and 5-digit quotes.

Broker stop-level restrictions can prevent a requested stop modification.

### 5. Equity protection

| Input | Default | What it controls |
|---|---:|---|
| `UseSafeEquity` | false | Enables basket equity protection |
| `SafeEquityRiskPercent` | 10.0 | Floating-loss threshold as a percentage of peak account equity |

#### UseSafeEquity

When enabled, the EA monitors the floating profit/loss of its positions for the current chart symbol and Magic Number.

If the basket's floating loss exceeds the configured threshold, the EA attempts to close that basket.

This feature is intended to limit the amount of floating loss tolerated by a basket.

#### SafeEquityRiskPercent

Defines the maximum floating-loss threshold as a percentage of the highest account equity recorded since the EA was initialized.

For example:

- Recorded peak account equity: $10,000
- SafeEquityRiskPercent: 10%
- Floating-loss threshold: $1,000

When the EA basket's floating loss exceeds $1,000, the equity-protection procedure is triggered.

A lower percentage triggers protection sooner. A higher percentage allows more floating loss before the EA attempts to close the basket.

This is an operational protection mechanism, not a guaranteed maximum-loss limit. Execution delays, slippage, gaps, and broker conditions can result in a different final loss.

### 6. Execution and identification

| Input | Default | What it controls |
|---|---:|---|
| `Slippage` | 3.0 | Maximum permitted execution deviation |
| `MagicNumber` | 999 | Identifier used to distinguish the EA's trades |

#### Slippage

Defines the permitted execution-price deviation.

A lower value is more restrictive and may result in more rejected or requoted trade requests.

A higher value allows greater execution-price deviation, which can result in a less favorable fill.

The input is converted into broker points according to the symbol's quote precision.

#### MagicNumber

Identifies the positions managed by a particular EA instance.

The EA uses the Magic Number together with the chart symbol to identify its own positions.

When running multiple instances, use different Magic Numbers for different strategies or independent baskets on the same symbol.

Using the same symbol and Magic Number across multiple instances can cause them to manage the same positions.

### 7. Understanding points and pips

Some legacy distance parameters retain the word `Pips` in their names even though their formulas use broker points.

| Input | Actual calculation unit |
|---|---|
| `GridStepPips` | Broker points |
| `TakeProfitPips` | Broker points |
| `TrailingStartPips` | Broker points |
| `TrailingStopPips` | Conventional pips, converted according to quote digits |
| `Slippage` | Conventional pips, converted according to quote digits |

For a typical five-digit Forex quote:

- 10 broker points = 1 conventional pip.
- 5 points = 0.5 pips.
- 150 points = 15 pips.
- 35 points = 3.5 pips.

These conversions should not be blindly applied to metals, indices, or other CFDs. Check the exact broker symbol specification, including digits, point size, tick size, tick value, and contract size.

Settings that work on one instrument may have completely different economic effects on another.

### 8. Dashboard and chart controls

The on-chart dashboard provides information about the EA's current state and its managed basket.

Depending on the platform, the dashboard displays:

- EA version, symbol, and timeframe.
- Algo Trading permission and license status.
- Account balance and equity.
- Floating P/L of the EA's current basket.
- Daily closed P/L for the current symbol and Magic Number.
- Number and total volume of BUY and SELL positions.
- Volume-weighted average entry price.
- Basket take-profit target.
- Estimated next grid-entry price and distance.
- Trailing-stop status.
- Equity-protection status and current account drawdown.
- Magic Number and current spread.
- Operational expiry information.

**Note:** Dashboard figures are monitoring information. Commission accounting can differ between MT4 and MT5 and from the broker's final statement.

#### Close All Orders

Clicking this button attempts to close the EA's open BUY and SELL market positions for the current chart symbol and Magic Number.

It does not close unrelated manual trades or positions belonging to another Magic Number. Pending orders are not handled by this button.

#### Direction button

Clicking the direction button cycles through:

1. Both
2. Buys Only
3. Sells Only
4. Both

The selected mode controls new initial entries and grid additions.

Changing the direction does not automatically close positions that are already open.

#### EA is Running / EA stopped

Clicking the green `EA is Running` button pauses the EA's own trading logic. The button turns red and displays `EA stopped`.

Clicking it again resumes operation.

Pausing the EA does not close open positions. However, while paused, its tick-based processing is suspended, including new grid entries, trailing-stop updates, and equity-protection checks.

Existing broker-side take-profit and stop-loss orders remain subject to normal broker execution. The dashboard continues to update.

The internal pause state is reset when the EA is initialized again.

#### Repositioning dashboard buttons

The three buttons are selectable chart objects and can be moved individually.

Select a button and drag it to the desired chart position. The background rectangle and information labels are not selectable, and the dashboard is not moved as one single object.

### 9. Strategy Tester tutorial

The Strategy Tester should be used to find suitable settings for each symbol and trading environment.

The supplied defaults are reference starting values only. They are not optimized presets and should not be assumed to be profitable.

Recommended testing procedure:

1. Select Matishko EA in the Strategy Tester.
2. Choose the intended symbol and chart timeframe.
3. Select a sufficiently long historical date range.
4. Use realistic tick modelling. On MT5, prefer Every tick based on real ticks when the broker provides suitable historical data.
5. Set the intended account deposit, leverage, and trading conditions.
6. Select the appropriate `TradeDirection`.
7. Start by testing position sizing and grid spacing.
8. Run additional tests using different values for the basket take-profit and risk-protection settings.
9. Compare net profit, equity drawdown, maximum basket size, margin usage, and the duration of floating losses.
10. Validate promising settings on a separate historical period.
11. Forward-test the selected configuration on a demo account.

On MT4, use Common → Positions = Long & Short when you want the EA's own direction input to control which side is permitted.

Avoid selecting a configuration based solely on the highest historical profit. A Martingale grid can produce frequent small winning baskets while hiding the risk of rare, substantial losses.

### 10. Important platform and risk information

**MT4 and MT5**

Both versions use the same 15 input names and defaults.

The MT5 version requires a hedging account. Netting accounts are not supported by the multi-position strategy.

**Expiry**

The current build has an operational expiry of 31 December 2028 at 23:59:59 broker-server time. After expiry, new entries are disabled while existing-position management remains available.

**Fixed stop loss**

There is no user-configurable fixed stop-loss input. The EA's basket take-profit, optional per-position trailing stop, and optional equity protection are separate mechanisms.

**Trading risk**

Matishko EA uses progressive grid entries and Martingale position sizing. Position volume can grow geometrically as more positions are added.

A prolonged adverse market movement can increase floating losses and margin usage significantly and may result in stop-out or loss of the trading balance.

No setting guarantees recovery, profitability, or protection against all market conditions.

Always test the exact build on a demo account and review the broker's symbol specifications before live use.
