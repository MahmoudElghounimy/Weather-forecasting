
### **Weather Forecasting with Prophet and Visualizations using Python**

This script demonstrates how to load, visualize, and forecast weather-related data (temperature, humidity, and wind speed) for the city of Delhi using **Python**. The steps involve data loading, exploratory data analysis (EDA), visualization using Plotly, and time-series forecasting using **Prophet** from Facebook. The project is divided into the following key sections:

### **1. Importing Libraries**

The script begins by importing essential libraries for data manipulation, visualization, and forecasting:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
from prophet import Prophet
```

- **`pandas`** is used for data manipulation and handling CSV files.
- **`numpy`** is used for numerical operations.
- **`matplotlib`** and **`seaborn`** are used for static data visualization.
- **`plotly.express`** is used for interactive visualizations.
- **`prophet`** is used for time-series forecasting.

### **2. Loading and Exploring the Data**

The dataset is loaded using **pandas** from a CSV file (`'DailyDelhiClimateTrain.csv'`). After loading the data, a few basic data exploration steps are carried out:

```python
data = pd.read_csv('DailyDelhiClimateTrain.csv')

print(data.head())
print(data.describe())
print(data.info())
```

- **`data.head()`**: Displays the first few rows of the dataset to give an overview.
- **`data.describe()`**: Provides statistical summaries (mean, standard deviation, min, max, etc.) of the numerical columns.
- **`data.info()`**: Displays information about the data types, non-null values, and memory usage.

### **3. Visualizing Weather Trends with Plotly**

The script uses **Plotly** to create interactive line charts to visualize the trends in temperature, humidity, and wind speed over time.

```python
figure = px.line(data, x="date", y="meantemp", title='Mean Temperature in Delhi over the years')
figure.show()

figure = px.line(data, x="date", y="humidity", title='Mean Humidity in Delhi over the years')
figure.show()

figure = px.line(data, x="date", y="wind_speed", title='Mean Windspeed in Delhi over the years')
figure.show()
```

- These plots help us understand the overall trends and variations in temperature, humidity, and wind speed in Delhi over the years.

### **4. Analyzing the Relationship Between Temperature and Humidity**

Next, a **scatter plot** is created to examine the relationship between temperature (`meantemp`) and humidity (`humidity`):

```python
figure = px.scatter(data_frame=data, x="humidity", y="meantemp", size="meantemp", trendline="ols", title="Relationship between Temperature and Humidity")
figure.show()
```

- This plot shows how humidity correlates with temperature, with a trendline (OLS regression) added to the plot to better visualize the relationship.

### **5. Preparing Data for Time-Series Forecasting**

The data is then preprocessed for use with **Prophet**, a time-series forecasting tool. Prophet expects the data to have two columns: `ds` for dates and `y` for the target variable (in this case, temperature):

```python
data["date"] = pd.to_datetime(data["date"], format="%Y-%m-%d")
data["year"] = data["date"].dt.year
data["month"] = data["date"].dt.month
```

- The **date** column is converted to a `datetime` object for easier manipulation.
- Additional columns, **year** and **month**, are extracted from the date for further analysis.

```python
forecast_data = data.rename(columns={"date": "ds", "meantemp": "y"})
print(forecast_data)
```

- The dataset is renamed to meet Prophet's expectations: `date` becomes `ds` and `meantemp` becomes `y`.

### **6. Time-Series Forecasting with Prophet**

Prophet is then used to forecast future temperatures. The following steps are taken:

1. **Model Initialization**: A Prophet model is created.

```python
model = Prophet()
model.fit(forecast_data)
```

2. **Forecasting**: The model is used to make predictions for the next 365 days (1 year).

```python
forecasts = model.make_future_dataframe(periods=365)
predictions = model.predict(forecasts)
```

3. **Visualization**: The forecasted results are plotted interactively using **Plotly**.

```python
plot_plotly(model, predictions)
```

- **`make_future_dataframe(periods=365)`** creates a dataframe that includes the historical data along with future dates (365 days in this case).
- **`model.predict(forecasts)`** generates the forecast.
- **`plot_plotly`** plots the forecasted results interactively.

