# Installation — v1.25

## MetaTrader 4

1. Download `Matishko_EA_v1.25.ex4` from the [v1.25 release](https://github.com/kzlca/Matishko-EA/releases/tag/v1.25).
2. Open MetaTrader 4 and select **File → Open Data Folder**.
3. Open `MQL4/Experts/` and copy the `.ex4` file there.
4. Return to MetaTrader 4. Right-click **Expert Advisors** in Navigator and select **Refresh**, or restart the terminal.
5. Attach Matishko EA to the intended chart and review all 15 inputs, including `TradeDirection`.

## MetaTrader 5

1. Download `Matishko_EA_v1.25.ex5` from the [v1.25 release](https://github.com/kzlca/Matishko-EA/releases/tag/v1.25).
2. Confirm the trading account uses **hedging mode**. Netting accounts are not supported.
3. Open MetaTrader 5 and select **File → Open Data Folder**.
4. Open `MQL5/Experts/` and copy the `.ex5` file there (or into a subfolder).
5. Return to MetaTrader 5. Refresh **Expert Advisors** in Navigator or restart the terminal.
6. Attach Matishko EA to the intended chart and review all 15 inputs, including `TradeDirection`.

## Trade direction

Both platforms provide the same `TradeDirection` input:

- `Both` — allow new BUY and SELL entries.
- `Buy Only` — allow only new BUY initial entries and grid additions.
- `Sell Only` — allow only new SELL initial entries and grid additions.

For MT4 Strategy Tester, set **Common → Positions** to **Long & Short** when you want the EA input to be the only direction restriction. The tester-level setting is separate from the EA input.

The direction input sets the starting mode shown by the chart control. Changing the chart control changes the runtime mode; reinitializing the EA restores the input value. Existing positions continue to be managed after a direction change.

## Updating

Before replacing an existing build, save your current settings and note the version in use. Remove the previous EA instance from the chart, replace the binary, refresh Navigator, and attach the new version. Review all inputs after updating; defaults may change between releases.

Always test the exact release on a demo account with the intended broker before considering live use.
