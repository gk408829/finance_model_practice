# Financial News Sentiment Analysis Trading System

This project implements a trading strategy based on sentiment analysis of financial news headlines. It analyzes the emotional tone of news related to a specific stock, generates trading signals based on sentiment scores, and backtests the strategy against historical price data.

## Features

- **News Sentiment Analysis**: Uses NLTK's VADER (Valence Aware Dictionary and sEntiment Reasoner) to analyze the sentiment of financial news headlines
- **Trading Signal Generation**: Converts sentiment scores into buy/sell/hold signals based on customizable thresholds
- **Backtesting Framework**: Tests strategy performance against historical data
- **Performance Metrics**: Calculates key metrics including returns, Sharpe ratio, and maximum drawdown
- **Visualization**: Plots stock price, sentiment scores, trading signals, and strategy performance

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/financial-sentiment-trading.git
cd financial-sentiment-trading

# Create and activate a virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install required packages
pip install -r requirements.txt
```

## Requirements

- Python 3.8+
- pandas
- numpy
- matplotlib
- yfinance
- nltk
- scikit-learn
- seaborn
- beautifulsoup4
- requests

## Usage

```python
from sentiment_analyzer import FinancialNewsSentimentAnalyzer

# Initialize the analyzer with a stock ticker and optional date range
analyzer = FinancialNewsSentimentAnalyzer(
    ticker='AAPL',
    start_date='2023-01-01',
    end_date='2023-12-31'
)

# Run the complete analysis workflow
trading_data, metrics, fig = analyzer.run_analysis()

# Display the visualization
import matplotlib.pyplot as plt
plt.show()
```

## Example Output

The system generates several outputs:

1. **Trading Data**: A DataFrame containing stock prices, sentiment scores, and trading signals
2. **Performance Metrics**: Summary statistics including:
   - Win rate
   - Cumulative return
   - Buy & hold return comparison
   - Maximum drawdown
   - Volatility
   - Sharpe ratio
3. **Visualization**: Three charts showing:
   - Stock price with sentiment overlay
   - Trading signals (buy/sell/hold)
   - Strategy performance compared to buy & hold

## Customization

You can customize the strategy by adjusting parameters:

```python
# Change the sentiment threshold for signal generation (default: 0.2)
trading_data = analyzer.generate_trading_signals(threshold=0.3)

# Adjust the lookback period for news headlines (default: 30 days)
analyzer.fetch_news_headlines(days=60)
```

## Current Limitations

- The example implementation uses synthetic news data. For production use, you'll need to connect to a real financial news API.
- The sentiment analyzer is not specifically trained on financial text, which may limit its accuracy in this domain.
- The backtesting framework does not account for transaction costs or slippage.

## Future Improvements

- Integrate with a real financial news API (NewsAPI, Finnhub, Alpha Vantage, etc.)
- Fine-tune the sentiment analyzer on financial text
- Implement a more sophisticated signal generation strategy
- Add portfolio-level backtesting for multiple stocks
- Incorporate additional features like volume data or technical indicators
- Add risk management rules

## License

MIT

## Acknowledgments

- NLTK team for the VADER sentiment analysis tool
- yfinance for providing free stock data access