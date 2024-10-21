import alpaca_trade_api as tradeapi
import time

# Set your API keys and endpoint
API_KEY = 'YOUR_API_KEY'
API_SECRET = 'YOUR_API_SECRET'
BASE_URL = 'https://paper-api.alpaca.markets'  # Use paper trading for testing

# Initialize the Alpaca API
api = tradeapi.REST(API_KEY, API_SECRET, BASE_URL, api_version='v2')

# Define your trading parameters
symbol = 'AAPL'  # Stock symbol
buy_price = 150.00  # Price threshold to buy
quantity = 1  # Number of shares to buy

def check_price_and_trade():
    # Get the latest price of the stock
    barset = api.get_barset(symbol, 'minute', limit=1)
    latest_price = barset[symbol][0].c
    print(f"Latest price of {symbol}: ${latest_price}")

    # Check if the latest price is less than the buy price
    if latest_price < buy_price:
        print(f"Buying {quantity} shares of {symbol} at ${latest_price}")
        api.submit_order(
            symbol=symbol,
            qty=quantity,
            side='buy',
            type='market',
            time_in_force='gtc'
        )
    else:
        print(f"No trade executed. Current price ${latest_price} is above buy threshold ${buy_price}.")

# Main trading loop
if __name__ == "__main__":
    while True:
        check_price_and_trade()
        time.sleep(60)  # Wait for 60 seconds before checking again
        
