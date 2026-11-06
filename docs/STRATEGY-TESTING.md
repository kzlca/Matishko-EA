# Strategy testing and parameter optimization

## Important: the shipped defaults are not optimized settings

The input defaults distributed with Matishko EA are **reference starting values only**. They have not been established through a systematic historical backtest, parameter optimization, or broker-specific forward test. They should not be interpreted as recommended, profitable, low-risk, or universally suitable settings.

There is no single best parameter set for every symbol, timeframe, broker, spread, commission schedule, swap schedule, leverage, or market regime. Users should test and evaluate their own parameter sets before using the EA.

## 1. Define the intended use first

Matishko EA is a Martingale-based grid manager. It is intended to manage a basket after a trader or an independent strategy has identified a possible direction.

Before testing, define:

- The symbol and timeframe on which the directional signal will be generated.
- Whether the EA will be restricted to BUY-only or SELL-only operation, or allowed to use both directions.
- The broker account type, leverage, deposit currency, and intended account size.
- The maximum loss, drawdown, and margin usage that would make the setup unacceptable.

Test the EA with the same symbol, timeframe, direction mode, and intended signal workflow that will be used in practice. A test of the EA by itself does not validate an external signal strategy.

## 2. Use realistic historical data and trading costs

Use the best-quality historical data available for the selected symbol. Grid entries, basket exits, and trailing stops depend on price movement inside bars, so coarse modelling can materially distort results.

- **MT4:** select the most detailed tick modelling available for the data you have. Check the report for modelling quality and missing-history warnings.
- **MT5:** prefer **Every tick based on real ticks** when the broker provides sufficient real-tick history. Otherwise, document the selected modelling mode and its limitations.
- Use realistic variable spreads where supported.
- Include commission, swap, contract size, tick value, volume step, margin requirements, and the intended leverage.
- Consider execution delay, slippage, gaps, market closures, and rejected trade requests. Backtests cannot reproduce every live execution condition.

## 2A. Include gold and other higher-volatility instruments as separate test candidates

**XAUUSD (gold)** may be worth evaluating alongside Forex symbols, particularly when the independent directional strategy is designed for that instrument. Treat this as a test candidate only; there is no evidence that the EA inherently performs better on gold. Gold is commonly offered as a metal CFD, not a Forex currency pair, and its trading conditions are broker-specific.

Before testing gold, inspect the exact symbol specification in Market Watch / Contract Specification. Record at least:

- Exact broker symbol name, including any suffix (for example, XAUUSD, GOLD, or a broker-specific variant).
- Digits, point size, tick size, and tick value in the account deposit currency.
- Contract size, minimum lot, maximum lot, volume step, and margin requirements.
- Typical and variable spread, commission, long/short swap, and trading sessions.
- Stop and freeze levels, execution mode, and any symbol-specific trading restrictions.

The nominal quote price of an instrument is not a measure of suitability. A larger or faster price movement can trigger more grid entries and accelerate Martingale exposure. Ensure the grid distances are interpreted in the EA's legacy broker-point units; do not assume the same numerical inputs represent the same economic risk across different symbols. Optimize and validate each instrument independently, with conservative exposure limits and a separate out-of-sample period.

Official platform guidance: [MT4 Strategy Tester](https://www.metatrader4.com/en/trading-platform/help/overview/strategy_tester) · [MT5 Strategy Testing](https://www.metatrader5.com/en/terminal/help/algotrading/testing).

## 3. Optimize a small number of parameters at a time

Do not optimize every input simultaneously. Start with the parameters that define the basket's exposure and spacing:

1. `LotSizeMultiplier` and `InitialLots` — position-volume growth and starting exposure.
2. `GridStepPips` and `DistanceMultiplier` — entry spacing and how quickly the grid expands.
3. `MaxOpenOrders` — the maximum number of simultaneous basket positions.
4. `TakeProfitPips` — basket exit distance.
5. `SafeEquityRiskPercent` and trailing-stop inputs — optional protective and exit behavior.

Use sensible ranges for the specific symbol and broker. Preserve the documented legacy point/pip behavior of the distance inputs; do not assume every input ending in Pips is measured in conventional Forex pips. See [Input parameters](INPUT-PARAMETERS.md).

Avoid selecting a setup solely because it has the highest net profit. With a Martingale grid, a backtest can show frequent small gains while concealing rare, very large basket losses.

## 4. Evaluate risk, not just return

Review the full report and trade history. In addition to net profit and profit factor, examine:

- Maximum balance and equity drawdown, in both money and percentage terms.
- Minimum margin level and the highest margin usage.
- Largest basket loss and the largest number of simultaneously open positions.
- Longest time a basket remained open and the duration of deep floating drawdowns.
- Total volume accumulated during adverse movement.
- Number of trades and baskets; a result based on very few baskets is weak evidence.
- Performance separately in rising, falling, ranging, and volatile market periods.

Reject parameter sets whose drawdown, margin usage, or maximum basket exposure exceed limits you can tolerate, even if their net profit is attractive.

## 5. Use separate in-sample and out-of-sample periods

Do not optimize and evaluate a parameter set on exactly the same historical period.

- Use an earlier period for initial testing and optimization.
- Reserve a later, unseen period for out-of-sample validation.
- In MT5, the Strategy Tester provides a Forward testing option for comparing optimized passes on a later period.
- In MT4, run a separate test on a later date range using the selected parameters.

Prefer parameter ranges that behave reasonably across nearby values and different periods. A single isolated parameter combination with exceptional historical profit can be a sign of overfitting.

## 6. Forward-test on demo

After historical testing, run the selected configuration on a demo account with the intended broker, account type, symbol, and timeframe. Confirm that order sizing, grid spacing, basket take-profit, trailing stops, Safe Equity, and direction controls behave as expected.

Compare demo execution with the assumptions used in the tester. Do not move to a live account merely because one backtest or optimization pass looks profitable.

## 7. Important limitations

Optimization cannot remove the structural risk of Martingale or grid trading. A sufficiently long adverse market move, gap, spread expansion, margin change, or execution failure can cause substantial losses, including loss of the account balance.

Historical results do not guarantee future performance. The user is responsible for selecting settings, validating the complete directional-signal workflow, and deciding whether the resulting exposure is acceptable.
