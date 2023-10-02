import ccxt
import numpy as np

# Initialize the exchange (replace 'binance' with your exchange)
exchange = ccxt.binance({
    'apiKey': '<your-api-key>',
    'secret': '<your-api-secret>',
})

# Define your trading parameters
symbol = 'BTC/USD'
amount = 0.01

# Define your strategy parameters
window_size = 100
threshold = 0.01

while True:
    # Fetch the recent price data
    data = exchange.fetch_ohlcv(symbol, '1m', limit=window_size)
    close_prices = [x[4] for x in data]

    # Calculate the moving average
    moving_average = np.mean(close_prices)

    # Fetch the current price
    current_price = close_prices[-1]

    # If the current price is significantly higher than the moving average, place a buy order
    if current_price > moving_average * (1 + threshold):
        order = exchange.create_market_buy_order(symbol, amount)
        print(f'Placed a buy order at {current_price}, order id: {order["id"]}')

