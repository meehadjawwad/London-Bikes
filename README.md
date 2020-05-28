# London Bikes

## Aim of the Project
The aim of this project was threefold:
- To identify patterns in the spread of the number of bike shares in London.
- To evaluate the relationship between weather, time, and the number of bike shares.
- To assess whether a statistical model can predict the number of bike shares.

## Table of Contents
1. [Hardware Used](https://github.com/meehadjawwad/London-Bikes#hardware-used)
2. [File Descriptions](https://github.com/meehadjawwad/London-Bikes#file-descriptions)
3. [Methods Used](https://github.com/meehadjawwad/London-Bikes#methods-used)
4. [Technologies Used](https://github.com/meehadjawwad/London-Bikes#technologies-used)
5. [Executive Summary](https://github.com/meehadjawwad/London-Bikes#executive-summary)
    * [The Data](https://github.com/meehadjawwad/London-Bikes#the-data)
    * [Data Cleaning](https://github.com/meehadjawwad/London-Bikes#data-cleaning)
    * [Exploratory Data Analysis](https://github.com/meehadjawwad/London-Bikes#exploratory-data-analysis)
    * [Feature Engineering](https://github.com/meehadjawwad/London-Bikes#feature-engineering)
    * [Modelling](https://github.com/meehadjawwad/London-Bikes#modelling)
    * [Conclusion & Further Steps](https://github.com/meehadjawwad/London-Bikes#conclusion--further-steps)

## Hardware Used
- MacBook Pro (Retina, 13-inch, Early 2015)
- Processor: 2.9 GHz Dual-Core Intel Core i5
- Memory: 8 GB 1867 MHz DDR3
- OS: macOS Catalina (version 10.15.3)

## File Descriptions
- preprocessing.ipynb: notebook for initial data cleaning, exploration, and analysis.
- modelling.ipynb: notebook for feature engineering, machine learning, and predictions.
- requirements.txt: includes a list of python libraries used in the project.
- data:
  - london_merged.csv: raw data file
  - london_merged_processed.csv: cleaned data
  - london_merged_modelling.csv: cleaned data, after feature engineering

## Methods Used
- Data cleaning
- Data exploration and visualisation
- Feature engineering
- Machine Learning

## Technologies Used
- Python
- Numpy
- Pandas
- Matplotlib
- Seaborn
- Statsmodels
- Scikit-learn
- Yellowbrick

## Executive Summary
As stated earlier, there were three aims of this project:
- To identify patterns in the spread of the number of bike shares in London.
- To evaluate the relationship between weather, time, and the number of bike shares.
- To assess whether a statistical model can predict the number of bike shares.

### The Data
The data, acquired from [Kaggle](https://www.kaggle.com/hmavrodiev/london-bike-sharing-dataset), is an hourly data of bike sharing counts, temperature, humidity, wind speed, season, holidays, and so on. The data (17,414 rows) spans over 2 years, from 2015 to 2017.

### Data Cleaning
The numerical weather and season codes in the data were replaced with appropriate labels, and some new columns were created (_Time, Day of the Week, Week Number, Month_) out of the original _timestamp_ column. The _holiday_ and _weekend_ columns were combined into a single _holiday_ column, due to the original _holiday_ column not having a significant amount of entries.

### Exploratory Data Analysis
I evaluated the representation of various columns (such as _season, weather, holidays_) within the data, through multivariate (non-graphical) analysis, and was happy with the representation.

Next, the data was plotted in order to assess any interesting patterns.

_Figure 1_:

![Fig. 1](https://github.com/meehadjawwad/London-Bikes/blob/master/images/bikes-time.png)

_Figure 1_ represents the overall pattern of how the number of bike shares is spread throughout the day. It can be observed that the number of bike shares first peaks at around 8 a.m. (which is probably due to the morning work commute) and then peaks again at around 5 and 6 p.m. (which is probably due to the evening work commute).

_Figure 2_:

![Fig. 2](https://github.com/meehadjawwad/London-Bikes/blob/master/images/bikes-weekends.png)

Fig. 2 represents the same data as in Fig. 1 but differentiated between weekdays and weekends (and holidays). An interesting observation is that whereas the spread on weekdays mimics the spread in Fig. 1, the spread on weekends and holidays has a different pattern. It spreads smoothly like a bell-curve, gently peaking at 2 and 3 p.m.

_Figure 3_:

![Fig. 3](https://github.com/meehadjawwad/London-Bikes/blob/master/images/bikes-months.png)

_Figure 3_ represents how the number of bike shares are spread throughout the year. It can be observed that the spread follows a smooth bell-curve, peaking during summertime in July.

Next, correlations between the number of bike shares and other variables were assessed.

_Figure 4_:

![Fig. 4](https://github.com/meehadjawwad/London-Bikes/blob/master/images/bikes-temp.png)

_Figure 4_ represents the correlation between bikes and temperature, and suggests an overall positive correlation with more variance during extreme temperatures.

_Figure 5_:

![Fig. 5](https://github.com/meehadjawwad/London-Bikes/blob/master/images/bikes-humidity.png)

_Figure 5_ represents the correlation between bikes and humidity, and suggests an overall negative correlation.

### Feature Engineering
In order to transform their qualitative nature, dummy variables were created for the _weather, season, time, day, and month_ columns. The data types, and some abnormalities within the _count_ column were corrected.

### Modelling
The data was firstly split into two sub-datasets, train (14,914 rows) and test (2,500) rows. Within the train sub-dataset, 5 KFold splits were created to train and validate the models. _R-squared (coefficient of determination)_ was chosen to be the scoring metric to assess the accuracy of the models.

The models developed were as follows:
1. Linear Regression
2. Lasso Regression
3. Ridge Regression
4. ElasticNet Regression

In order to determine the best alpha value for the Lasso, Ridge, and ElasticNet models, the models were looped through a list of alpha values and their R-squared values were recorded, along with the number of variables that the models found useful. 

_Figure 6_:

![Fig. 6](https://github.com/meehadjawwad/London-Bikes/blob/master/images/models.png)

_Figure 6_ represents the four best performing models.

The Lasso model was selected due to its simplicity (lesser number of features), and was tested against the test sub-dataset. The residuals resulting from the predictions made through the model suffered from _heteroscedasticity (a condition where variance of the residuals increases as the response value increases)._

As a solution, the _count_ data was log-transformed (for both the train and test sub-datasets) and the modelling process was reimplemented.

_Figure 7_:

![Fig. 7](https://github.com/meehadjawwad/London-Bikes/blob/master/images/models2.png)

_Figure 7_ shows the four best performing models after the log-transformation. It must be noted that the R-squared scores were improved due to the transformation. Again, the Lasso model was selected due to its simplicity, and was tested against the test sub-dataset.

_Figure 8_:

![Fig. 8](https://github.com/meehadjawwad/London-Bikes/blob/master/images/logresid.png)

_Figure 8_ represents the final residual plot. The residuals were greatly improved in comparison to the previous model, but there is still room for improvement for higher response values.

### Conclusion & Further Steps
* There are daily and yearly patterns to how the number of bike rides are spread. The spread is also different for weekdays and weekends.
* There are linear relationships between the target and the features; and linear models can reach an accuracy score of around 83%
* The models have trouble predicting higher values.

Further Steps:
* Due to its nature, the data has high multicollinearity, which needs to be dealt with in order the improve the models.
* A polynomial regression model must also be built, which would require further feature engineering in order to reduce the number of features fed into the model.
* ARIMA, SARIMA, and VAR forecasting on the data would also be interesting and useful step.
