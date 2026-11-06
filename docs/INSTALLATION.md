# Installation

## MetaTrader 4

1. Download `Matishko_EA_v1.24.ex4` from the [v1.24 release](https://github.com/kzlca/Matishko-EA/releases/tag/v1.24).
2. Open MetaTrader 4 and select **File → Open Data Folder**.
3. Open `MQL4/Experts/` and copy the `.ex4` file there.
4. Return to MetaTrader 4. Right-click **Expert Advisors** in Navigator and select **Refresh**, or restart the terminal.
5. Attach Matishko EA to the intended chart and review all parameters.

## MetaTrader 5

1. Download `Matishko_EA_v1.24.ex5` from the [v1.24 release](https://github.com/kzlca/Matishko-EA/releases/tag/v1.24).
2. Confirm the trading account uses **hedging mode**. Netting accounts are not supported.
3. Open MetaTrader 5 and select **File → Open Data Folder**.
4. Open `MQL5/Experts/` and copy the `.ex5` file there (or into a subfolder).
5. Return to MetaTrader 5. Refresh **Expert Advisors** in Navigator or restart the terminal.
6. Attach Matishko EA to the intended chart and review all parameters.

## Updating

Before replacing an existing build, save your current settings and note the version in use. Remove the previous EA instance from the chart, replace the binary, refresh Navigator, and attach the new version. Review all inputs after updating; defaults may change between releases.

Always test the exact release on a demo account with the intended broker before considering live use.
