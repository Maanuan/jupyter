# Function to fetch data and calculate indicators
def fetch_data(ticker, period='1y', interval='1d'):
    # Fetch data from Yahoo Finance
    df = yf.download(ticker, period=period, interval=interval)
    
    # Calculate the 100-period Simple Moving Average (SMA)
    df['SMA_100'] = talib.SMA(df['Close'], timeperiod=100)
    
    # Calculate the 14-period RSI
    df['RSI_14'] = talib.RSI(df['Close'], timeperiod=14)
    
    return df

# Function to generate buy signals
def generate_signals(df):
    # Create a column for buy signals where RSI is greater than the 100-period SMA
    df['Buy_Signal'] = (df['RSI_14'] > df['SMA_100']).astype(int)
    return df

# Main execution
if _name_ == "_main_":
    # Define the stock ticker
    ticker = 'AAPL'  # Example stock
    
    # Fetch stock data
    data = fetch_data(ticker)
    
    # Generate buy signals
    data_with_signals = generate_signals(data)
    
    # Filter and display only buy signal days
    buy_days = data_with_signals[data_with_signals['Buy_Signal'] == 1]
    print(buy_days[['Close', 'SMA_100', 'RSI_14', 'Buy_Signal']])
