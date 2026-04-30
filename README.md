# Gold Price Forecasting: Comparing Time Series and Machine Learning Methods

**Author**: Github: lillion2003

This project covers the steps for forecasting gold prices using different methods, including Time Series and Machine Learning approaches. The goal is to compare how well each method can predict gold prices during the Russia-Ukraine war.

## Table of Contents

- [Problem](#problem)
- [Data](#data)
- [Methods](#methods)
- [Methodology](#methodology)
- [Results](#results)
- [Limitations](#limitations)
- [Recommendations & Next Steps](#recommendations--next-steps)
- [Project Structure](#project-structure)

## Problem

The Russia-Ukraine war has had a significant impact on the global economy, including geopolitical uncertainty, economic sanctions, energy price volatility, and inflationary pressure. These factors have pushed investors to move their money into assets seen as "safe havens" such as gold, causing gold prices to become more volatile and react to news more quickly and sharply. Under this situation, accurate gold price forecasting is very important for making investment decisions, managing risk, and setting economic policies.

**Research question:** Find the most suitable method for forecasting gold prices during the Russia-Ukraine war by comparing **Time Series methods versus Machine Learning methods**, in order to get a model that can be used in practice for supporting investment decisions or policy planning.

## Data

The data used is daily gold price data, which includes the Open price, High price, Low price, and Close price.

- This study builds the models using the Close price.

## Methods

The models used for forecasting are as follows:

**1. Time Series Method**

- **Holt's Exponential Smoothing**: A model suitable for data with a trend. It builds the model by giving more weight to recent data than older data.

- **ARIMA (AutoRegressive Integrated Moving Average)**: A popular time series model that uses past data to predict the future. It adjusts the model based on past values (p) and past errors (q).

**2. Machine Learning Method**

- **LSTM (Long Short-Term Memory)**: A neural network with memory cells that can be updated. It can learn both long-term and short-term patterns in the data effectively, making it suitable for time series data.

- **XGBoost (Extreme Gradient Boosting)**: An ensemble decision tree algorithm that uses multiple trees to make predictions together. When used for regression tasks, it usually gives high accuracy and can process data quickly.

## Tools

The tools and libraries used for analysis and model development are grouped by method as follows:

### Time Series Method (R)

For the time series analysis, we used the **R** language with the following main libraries:

- **forecast**: Used for building and forecasting Holt's Exponential Smoothing and ARIMA models.
- **tseries**: Used for testing stationarity of the data (Stationarity Test such as ADF test).
- **ggplot2**: Used for creating plots and data visualization.
- **Metrics**: Used for calculating error values and measuring model performance (such as RMSE, MAE, MAPE).
- **car, lmtest**: Used for hypothesis testing to check the adequacy of the ARIMA model.

### Learning Based Method (Python)

For the Machine Learning and Deep Learning analysis, we used **Python** with the following main libraries:

- **PyTorch**: Used for building and training Deep Learning models.
- **XGBoost**: Used for building the XGBoost model.
- **Skforecast**: A library for Time Series Forecasting using general Machine Learning models (used for XGBoost only).
- **Pandas**: Used for data manipulation.
- **NumPy**: Used for mathematical calculations.
- **Matplotlib**: Used for plotting graphs (Data Visualization).

## Methodology

### Time Series Model

**1. Holt's Exponential Smoothing**

Steps for building the model:

- Check the suitability by looking at the data characteristics.
- Estimate the parameters.
- Forecast and evaluate the results.

**2. ARIMA (AutoRegressive Integrated Moving Average)**

Steps for building the model:

- Test for stationarity using the ADF-Test.
- Check whether the data is stationary or not.
- Examine the ACF and PACF to determine the values of p and q.
- Test the significance of the model parameters.
- Specify the forecasting equation.
- Test the adequacy of the model.
- Forecast and evaluate the results.

### Machine Learning

**1. Modeling Approach**

**Lags Features**

We used lag features (past values) as input features, selected appropriately for each model. The past gold prices were used as independent variables for prediction.

![Lags Feature](Pictures/Lags%20Feature.png)

**Multi-Step Direct Forecasting**

We built the forecasting models using the Multi-Step Direct Forecasting strategy to make the comparison fair. A separate model was created for each time step to be predicted. The idea is to build 155 models, all trained on the same training data, to forecast gold prices for the next 155 days (using the same lag features as X, but setting different Y targets depending on the target day for each model).

![Multi Step Direct Forecasting](Pictures/Multi%20step%20direct%20forecasting.png)

**Hyperparameter Tuning**

For each Machine Learning method, we searched for the best set of hyperparameters using Grid Search with Rolling Window to get the best performing model.

![Rolling Window CV](Pictures/Rolling%20Window%20CV.png)

**2. Models Used**

- **LSTM (Long Short-Term Memory)**: A neural network with memory cells that can be updated, suitable for time series data. Built using the PyTorch library.

- **XGBoost (Extreme Gradient Boosting)**: An ensemble decision tree algorithm. Built using the Skforecast library, which is a Python library for Time Series Forecasting using general Machine Learning models.

**3. Evaluation**

The forecasting results from each method were compared using the following metrics:

- **RMSE (Root Mean Square Error)**: A metric that is sensitive to large errors, as they make the value grow quickly.
- **MAE (Mean Absolute Error)**: Measures the average error.
- **MAPE (Mean Absolute Percentage Error)**: Measures the error as a percentage.

## Results

The performance comparison between Time Series and Machine Learning methods is shown in the results file [Summarize Results.md](Summarize%20Results.md)

## Limitations

**Limitations of this study:**

1. **ARIMA(0,2,1)** did not pass the statistical adequacy tests. It was used only for performance comparison and should not be used for actual forecasting.

2. From multiple experiments, we found that LSTM is very sensitive to hyperparameters. This is probably because **deep learning models generally need a large amount of data**. On the other hand, XGBoost showed relatively stable accuracy, possibly because even though the data was small, it was enough to train the model.

3. **The data in this scenario is limited** in size, which may prevent the Machine Learning methods from showing their full potential.

4. **Validation design** in this study was designed to be as similar to the Test set forecasting as possible. We applied Multi-Step Direct strategy in the evaluation and set the evaluation period in each fold to 155 days (equal to the number of days in the Test set). Because of this, the 5-fold Rolling Window had to be designed with overlapping periods, which means the results from each fold may be correlated with each other (the data was not enough to make each fold completely separate).

   However, this comparison aims to find a good method for forecasting gold prices during crisis situations in the future. For forecasting in normal situations where more data can be collected, the hyperparameter results from the Machine Learning methods might be different, when the Rolling Window CV for hyperparameter search can be designed more properly.

5. **Machine Learning results** showed a clear gap between the Validation accuracy and the Test set performance. More data should be considered, or the Grid Search range should be adjusted.

6. **The hyperparameter sets** used in this study were set with basic differences in each search round. We could not make them more diverse due to limited computational resources. The hyperparameter search range should be expanded for more complete results.

## Recommendations & Next Steps

**Recommendations and future development directions:**

- **Model Selection**: Choose the model with the best performance on the Test set for real-world use.

- **Ensemble Methods**: Consider combining different methods together (Ensemble). For example, take the residuals from XGBoost predictions and fit them with another Machine Learning model to further reduce the residuals. This is a way to use 2 models together.

- **Real-time Implementation**: The best model can be developed into a real-time gold price forecasting system.

- **Data Collection**: Collect more data to improve the performance of the Machine Learning methods.

- **Exogenous Variables**: Adding external variables such as exchange rates, oil prices, or interest rates would be helpful. This can be done by switching to the ARIMAX model instead of ARIMA, or by adding more features to the Machine Learning models, which could help improve the forecasting accuracy.

## Project Structure

You can find all the code and analysis details in the related Jupyter Notebook files.

```
├── README.md                                    <- Project overview file
├── Lags Feature.png                             <- Image explaining Lags Features
├── Rolling Window CV.png                        <- Image explaining Rolling Window Cross-Validation
├── Multi step direct forecasting.png            <- Image explaining Multi-Step Direct Forecasting strategy
├── Result.png                                   <- Graph showing forecasting results on the Test set
│
├── Time Series Method/
│   ├── ARIMA.ipynb                              <- ARIMA analysis and forecasting
│   └── Holt's Exponential Smoothing.ipynb       <- Holt's ES analysis and forecasting
│
├── Learning Based/
│   ├── LSTM/
│   │   ├── LSTM Forecasting.ipynb               <- LSTM forecasting (using the best parameters)
│   │   └── LSTM grid search.ipynb               <- Hyperparameter search for LSTM
│   │
│   └── XGBoost/
│       ├── XGBoost Forecasting.ipynb            <- XGBoost forecasting (using the best parameters)
│       └── XGBoost grid search.ipynb            <- Hyperparameter search for XGBoost
│
└── Summarize Results.md                         <- Summary and comparison of all methods
```
