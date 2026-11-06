# Risk and limitations

## Martingale and grid risk

Matishko EA is a **Martingale-based grid strategy**. With a lot multiplier above 1, position volume can grow geometrically as more entries are added. If the market moves persistently against the basket, exposure and margin usage can rise quickly until the maximum-position limit, available margin, a broker restriction, or an enabled protective rule prevents further entries.

The basket's weighted-average entry price can move as positions are added, but this does not guarantee a market reversal or a profitable exit. Possible outcomes include:

- Rapid growth in total position volume and required margin.
- Large floating drawdowns and stop-out.
- Losses amplified by leverage, market gaps, slippage, widening spreads, and illiquid conditions.
- Different results between historical testing and live execution.
- Account-level exposure across multiple EA instances.

## Risk controls are not guarantees

Trailing stops, basket take-profit, maximum-position limits, and equity protection do not eliminate market or execution risk. Stops may execute at worse prices than requested. A terminal, network, or broker failure can interrupt management, and trade requests can be rejected. Equity-protection thresholds are operational controls, not guaranteed maximum-loss limits.

The MT5 build requires a hedging account. Netting mode is not supported by this multi-position design.

## Before live use

1. Test the exact compiled release on a demo account with the intended broker and symbol.
2. Review initial lot, lot multiplier, grid step, maximum positions, direction mode, stop settings, and equity-protection settings.
3. Verify broker volume limits, tick size/value, margin requirements, stop/freeze levels, spreads, commissions, and swaps.
4. Test terminal restart, disconnection, rejected orders, low-margin conditions, high volatility, and market gaps.
5. Use conservative settings and capital that can withstand substantial drawdown.

No profitability, capital-protection, or trading-stability guarantee is made. Use of the EA is at the user's own risk.
