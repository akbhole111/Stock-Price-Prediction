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

* **Purpose**: Statistical time-series model.
* **Why used**: Provides a solid **baseline** by modeling short-term autocorrelations in stock price movements.
* **Strengths**: Simple, interpretable, works well for **stationary time-series**.
* **Limitations**: Struggles with **long-term dependencies** and sudden market shocks.

---

### Prophet

* **Purpose**: Forecasting model developed by Facebook.
* **Why used**: Handles **trend, seasonality, and holidays** with minimal tuning.
* **Strengths**: Robust to **missing data and outliers**, easy to interpret results.
* **Limitations**: Less effective at capturing **high-frequency fluctuations** common in stock data.

---

### LSTM (Long Short-Term Memory Networks)

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
