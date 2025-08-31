# Stock-Price-Prediction
Predict future stock prices or the movement (up/down) of stocks using historical price data. This helps investors make informed decisions.

---

#  Stock Price Forecasting with ARIMA, Prophet & LSTM

##  Project Overview

This project compares **classical, statistical, and deep learning models** for stock price forecasting.
We use Apple (AAPL) stock data and evaluate the models:

* **ARIMA** – autoregressive model for short-term dependencies
* **Prophet** – trend + seasonality decomposition
* **LSTM** – recurrent neural network for long-term memory

The goal is to highlight how different approaches handle stock price prediction.

---

##  Dataset

* Source: Historical Apple (AAPL) stock prices
* Columns: `Date`, `Close`
* Preprocessing:

  * Converted `Date` to datetime
  * Sorted chronologically
  * Normalized prices for LSTM

---

##  Models & Justifications

### ARIMA (AutoRegressive Integrated Moving Average)

Step 1: Stationarity Check
ARIMA works best on stationary time series (no trend, constant mean/variance over time). So the first step is to check whether the stock closing price series is stationary. If not, you apply differencing to remove trends and make it stationary.

Step 2: Model Components
ARIMA is built from three terms:

* AR (p) – the autoregressive part, using past values to predict the future.
* I (d) – the integrated part, which involves differencing to make the series stationary.
* MA (q) – the moving average part, which uses past forecast errors to improve predictions.
So, the model is denoted ARIMA(p, d, q).

Step 3: Model Fitting
You choose p, d, q parameters (often using techniques like ACF/PACF plots or automated search). Then, ARIMA learns the relationship between past values and errors to predict future stock prices.

Step 4: Forecasting
Once trained, ARIMA produces forecasts for the next n steps. Since ARIMA is linear, the forecast is interpretable but may not capture very complex patterns in stock data.

* **Purpose**: Statistical time-series model.
* **Why used**: Provides a solid **baseline** by modeling short-term autocorrelations in stock price movements.
* **Strengths**: Simple, interpretable, works well for **stationary time-series**.
* **Limitations**: Struggles with **long-term dependencies** and sudden market shocks.

---

### Prophet (by Facebook/Meta)

Step 1: Data Preparation
The dataset is simplified into two columns:

* ds: dates in datetime format.
* y: closing prices (numeric).

Step 2: Model Components
Prophet models time series as a sum of:

* Trend – long-term movement (growth or decline).
* Seasonality – repeating cycles (weekly, yearly, daily).
* Holidays/Events – special dates that affect the time series (optional).

Step 3: Model Training
When fitting, Prophet automatically detects trend and seasonality patterns. It uses an additive model where these components are combined to explain the stock price movements.

Step 4: Forecasting
You extend the timeline into the future (e.g., next 30 days). Prophet generates predictions for those dates, along with uncertainty intervals.

Step 5: Visualization
Prophet provides clear plots:

* Forecast with actual vs predicted values.
* Component plots for trend and seasonality.
* Prophet is interpretable and fast but may not capture very sudden or highly non-linear movements compared to LSTM.

* **Purpose**: Forecasting model developed by Facebook.
* **Why used**: Handles **trend, seasonality, and holidays** with minimal tuning.
* **Strengths**: Robust to **missing data and outliers**, easy to interpret results.
* **Limitations**: Less effective at capturing **high-frequency fluctuations** common in stock data.

---

### LSTM (Long Short-Term Memory Networks)

Step 1: Data Preparation
Stock prices are sequential, so LSTM is ideal because it captures long-term dependencies.

* The closing prices are normalized (scaled to a small range like [0,1]).
* Data is split into training and testing sets.
* The series is then converted into input-output pairs, where past n days are used to predict the next day’s price.

Step 2: LSTM Architecture
LSTM is a type of recurrent neural network (RNN) that avoids the vanishing gradient problem by using memory cells and gates. These gates decide:

* What to keep from the past (long-term memory).
* What to forget (irrelevant history).
* What new information to add.
This makes LSTMs powerful for time-series forecasting compared to ARIMA.

Step 3: Model Training
The training process involves feeding the sequential input data (sliding windows of stock prices) into the LSTM layers. The network learns patterns of price movement, including non-linear ones.

* **Purpose**: A type of **Recurrent Neural Network (RNN)** designed for sequential data.
* **Why used**: Captures **long-term dependencies** in stock movements that ARIMA and Prophet may miss.
* **Strengths**: Learns **nonlinear and complex patterns** in stock prices.
* **Limitations**: Requires **more data and computational power**, less interpretable than ARIMA/Prophet.

---

## Evaluation

We used:

* **MAE (Mean Absolute Error):** average prediction error
* **RMSE (Root Mean Squared Error):** penalizes large deviations

---

## Results

| Model   | MAE  | RMSE | Notes                                       |
| ------- | ---- | ---- | ------------------------------------------- |
| ARIMA   | 1.89 | 2.54 | Best overall, strong at short-term trends   |
| Prophet | 4.27 | 5.44 | Medium accuracy, good at trend/seasonality but less precise    |
| LSTM    | 6.42 | 8.08 | Struggled with small dataset, overfit & less accurate        |

---

## Key Takeaways

* **ARIMA** → Outperformed Prophet & LSTM, best for short-term stock forecasting
* **Prophet** → Captured underlying trend & seasonality, but less accurate
* **LSTM** → Did not generalize well on limited data; could improve with larger datasets

## Qualitative Evaluation (Behavior & Interpretability)
#ARIMA
* Best if the stock data shows strong autocorrelation and linear trends.
* Easy to explain and justify predictions.
* Struggles with sudden market shocks or non-linear behavior.

#LSTM
* Best when the stock shows complex, non-linear, and long-term dependencies.
* Can adapt to intricate patterns ARIMA/Prophet miss.
* Harder to interpret (“black box” model).

#Prophet
* Best when the stock has clear trend + seasonality (like weekly or yearly effects).
* Very interpretable (you can directly see trend and seasonal components).
* Not as flexible for highly irregular or noisy stock data.

## Decision Process
* If interpretability is most important → Choose Prophet.
* If the stock data looks stationary and mostly linear → Choose ARIMA.
* If the stock shows complex, non-linear movements → Choose LSTM.
