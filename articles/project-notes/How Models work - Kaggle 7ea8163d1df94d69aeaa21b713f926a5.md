# How Models work - Kaggle

Start with decision tree.

A decision tree Auto with a question condition

If yes --> Do this

If no --> Do that

The process of capturing these patterns from a date set is called “**fitting" or "training"** your model

The data used to do this is called a **"training data"** The prediction at the bottom of the th decision tree is a leaf.

### Using Pandas

import pandas as pd

Pandas gives access to DataFrame. Df is a spreadsheet/table database.

The next step is to load your data using the filepath

pd.read-csv()

<aside>
💡 a good way to simplify this is

Store the filepath to a python variable

Store the loaded data as well to a variable

Eg weatherdata = filepath

Weatherdf = pd.read-csv(weatherdata)

</aside>

Call the .describe() method to get a summary description of the dataframe

Count, mean std, min, 23%, 50%, 75%, max

Suppose that our aim is to predict a value given the information we have in our dataframe, we must select a prediction target

**In processing and analysing our data, we will need to select subsets of that data.** There are 2 ways

1. Dot notation -> for predictors targets
2. Selecting column list -> select features

### Prediction targets and features.

**Dot notation**

df.name -> This calls the 'name' column in the df dataframe

The list of column values is called a Pandas series

When we want to predict a value using our dataset, we can set variable ‘y’ to the desired prediction target column using y = df.name

**Limitations**

Because the table name is called like an attribute, this method of accessing columns is called attribute access

The columns that can be called must conform to python syntax.

A better to call a column is df['name']

Features are all other columns with which we want to determine our prediction

set

x = df['name', 'age', 'room']

set dependent y and independent 'x' variable.

. describe() .head()

Pandas -> process data, set independent and dependent variables

Scikit-learn- Create Models using dataframes

Pandas -> Scikit-learn

### Model building steps

Define - define the type of model

Fit - Capture patterns in available data

Predict -

Evaluate - How accurate is the model?

**To create a model**

fist set & y = df['col']

then set x = df['col 1', 'col 2', 'col n']

Import

```jsx
# Data Loading Code Hidden Here
import pandas as pd

# Load data
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 
# Filter rows with missing price values
filtered_melbourne_data = melbourne_data.dropna(axis=0)
# Choose target and features
y = filtered_melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea', 
                        'YearBuilt', 'Lattitude', 'Longtitude']
X = filtered_melbourne_data[melbourne_features]

from sklearn.tree import DecisionTreeRegressor
# Define model
melbourne_model = DecisionTreeRegressor()
# Fit model
melbourne_model.fit(X, y)

DecisionTreeRegressor()
# Once we have a model, here is how we calculate the mean absolute error:

from sklearn.metrics import mean_absolute_error

predicted_home_prices = melbourne_model.predict(X)
mean_absolute_error(y, predicted_home_prices)
434.71594577146544
```

### Model Validation

After creating the model, model validation helps evaluate the quality of the model

A model’s quality is determined by its predictive accuracy. i.e how close to reality is the prediction.

<aside>
💡 Mean Absolute Error (MAE)

Find the error for each prediction: 

Error = Actual - Predicted

Convert error to positive by taking the absolute value

|Error|

Take the average of all errors:

|Error 1| + |Error 2| + |Error n| / n

</aside>

If you use a single training dataset to train a model, the system may find false correlations in the data that will always hold true within the sample dataset. But the model is only truly valuable if it can predict data in the real world

One way to handle this is to split the training data into two sets, train the model using one set and predict data in the other set

```jsx
train_test_split
```