# AlgoTrading
Portfolio of algorithms for trading the stock market

## Strategy: Renko RSI EMA Filter

This strategy, implemented in Pine Script, combines Renko charting with RSI and EMA filters to generate trading signals. The main features are:

- **Renko Chart Basis**: The strategy is designed to be used on Renko charts, which filter out minor price movements and focus on significant trends.
- **RSI Filter**: Uses a short-period RSI (default 2) to identify overbought (RSI > 70) and oversold (RSI < 30) conditions for trade entries.
- **EMA Trend Filter**: Utilizes three EMAs (default 10, 20, 30) to determine the prevailing trend. Long trades are only taken when EMAs are aligned bullishly (EMA10 > EMA20 > EMA30), and short trades when aligned bearishly (EMA10 < EMA20 < EMA30).
- **ATR-based Position Sizing**: Position size is dynamically calculated based on ATR, risk percentage, and contract value, aiming to standardize risk per trade.
- **Date Range Filter**: Trades are only taken within a user-defined date range.
- **Trade Length Table**: Optionally displays a table summarizing the distribution of trade lengths (in Renko bricks), risk/reward, and probabilities, helping to analyze trade outcomes.
- **Visual Aids**: Plots entry signals and colors the background based on position direction.

### How it works
1. **Entry Conditions**:
   - Long: RSI > 70, bullish EMA alignment, within date range.
   - Short: RSI < 30, bearish EMA alignment, within date range.
2. **Exit Conditions**:
   - Long: RSI drops below 70 or out of date range.
   - Short: RSI rises above 30 or out of date range.
3. **Position Sizing**: Number of contracts is calculated to risk a fixed percentage of capital per trade, based on ATR and contract value.
4. **Trade Analysis Table**: When enabled, shows statistics on trade lengths and risk/reward distribution.

This strategy is intended for research and educational purposes. Parameters can be adjusted to fit different markets or risk profiles.

---

## Automating the Strategy with PickMyTrade and Webhooks

You can automate trade execution for this strategy using [PickMyTrade](https://pickmytrade.trade/) by sending webhook alerts from TradingView. Below are the general steps:

### 1. Create a PickMyTrade Account
- Sign up at [PickMyTrade](https://pickmytrade.trade/) and set up your broker/API integration as per their documentation.

### 2. Prepare Webhook URLs
- In PickMyTrade, obtain your unique webhook URL for sending trade signals.

### 3. Add Alert Conditions in TradingView
- Open your Renko RSI EMA Filter strategy in TradingView.
- Click on the "Add Alert" button.
- Set the alert condition to trigger on your entry/exit signals (e.g., `strategy.entry` or `strategy.close`).

### 4. Configure Webhook Message
- In the alert creation window, enable the "Webhook URL" option and paste your PickMyTrade webhook URL.
- Use a JSON message format that PickMyTrade expects. Example:
  ```json
  {
    "symbol": "{{ticker}}",
    "side": "buy", // or "sell" or "close"
    "quantity": "{{strategy.order.contracts}}",
    "price": "{{close}}"
  }
  ```
- Adjust the fields as required by your broker and PickMyTrade's API.

### 5. Test the Integration
- Trigger a test alert in TradingView and verify that the order appears in PickMyTrade and is sent to your broker.

### 6. Go Live
- Once tested, enable alerts for live trading. Monitor both TradingView and PickMyTrade dashboards for order status and logs.

**Note:** Always test with a demo account or paper trading before using real funds. Refer to PickMyTrade's documentation for advanced features and troubleshooting.

---

## Suggestions for Improving Profitability

While the Renko RSI EMA Filter strategy is trend-following and performs well in trending markets, it may experience drawdowns during rangebound or sideways market regimes (such as summer months). To mitigate this and improve overall profitability, consider the following enhancements:

- **Combine with Mean Reversion Strategies:**
  - Implement a mean reversion strategy that activates when the market is rangebound. This can help capture profits from price oscillations within a range, offsetting losses from trend strategies during non-trending periods.
  - Use indicators like Bollinger Bands, RSI (with different thresholds), or moving average envelopes to identify and trade mean reversion setups.
- **Market Regime Detection:**
  - Develop logic to detect when the market is trending versus rangebound (e.g., using ADX, volatility filters, or price structure analysis).
  - Dynamically switch between trend-following and mean reversion strategies based on detected regime.
- **Diversification:**
  - Apply the strategy to multiple symbols or timeframes to reduce risk and smooth equity curves.
- **Parameter Optimization:**
  - Regularly review and optimize strategy parameters for different market conditions.

By combining trend-following and mean reversion approaches, you can create a more robust trading system that adapts to changing market environments and reduces drawdowns during periods of low volatility or sideways movement.


