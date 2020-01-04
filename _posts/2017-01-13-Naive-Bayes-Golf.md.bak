--- 
layout: post 
title:   Naive Bayes Example using Golf Dataset
author: Lewis Gavin 
comments: true 
tags: 
- data science
- machine learning
---

The following notebook works through a really simple example of a Naive Bayes implementation. 

The aim of this machine learning application is to predict whether or not to play golf based on Weather conditions.

## 1. Import the required Libraries


```python
import pandas as pd
import numpy
from sklearn import cross_validation
from sklearn.naive_bayes import GaussianNB
```

## 2. Read in the Data file

Here we are going to read in the golf.csv data file using the pandas library. This will read our CSV file into a pandas data frame.


```python
golf_file = "Golf.csv"

# Open the file for reading and read in data
golf_file_handler = open(golf_file, "r")
golf_data = pd.read_csv(golf_file_handler, sep=",")
golf_file_handler.close()

golf_data.head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Row No</th>
      <th>Outlook</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Wind</th>
      <th>Play</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Sunny</td>
      <td>85</td>
      <td>85</td>
      <td>False</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Sunny</td>
      <td>80</td>
      <td>90</td>
      <td>True</td>
      <td>No</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Overcast</td>
      <td>83</td>
      <td>78</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Rain</td>
      <td>70</td>
      <td>96</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Rain</td>
      <td>68</td>
      <td>80</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Rain</td>
      <td>65</td>
      <td>70</td>
      <td>True</td>
      <td>No</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Overcast</td>
      <td>64</td>
      <td>65</td>
      <td>True</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Sunny</td>
      <td>72</td>
      <td>95</td>
      <td>False</td>
      <td>No</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Sunny</td>
      <td>69</td>
      <td>70</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>Rain</td>
      <td>75</td>
      <td>80</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
  </tbody>
</table>
</div>



## 3. Data Cleansing and Feature Selection

As with any Data Science application, data cleansing and feature selection play a vital role. 

1. We need to select the columns from the dataset that we feel will give us the best prediction score - this is called feature selection. Here we will use all columns apart from the first one, as this is a row number column.
2. We need to ensure the data is in the correct format for the naive bayes algorithm. 
    1. We need to map our string column Outlook to numbers. This is because the naive bayes implementation cannot deal with strings.


```python
# Remove the Row No column as it is not an important feature
golf_data = golf_data.drop("Row No", axis=1)

# Map string vales for Outlook column to numbers
d = {'Sunny': 1, 'Overcast': 2, 'Rain': 3}
golf_data.Outlook = [d[item] for item in golf_data.Outlook.astype(str)]

golf_data.head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Outlook</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Wind</th>
      <th>Play</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>85</td>
      <td>85</td>
      <td>False</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>80</td>
      <td>90</td>
      <td>True</td>
      <td>No</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>83</td>
      <td>78</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>70</td>
      <td>96</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>68</td>
      <td>80</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3</td>
      <td>65</td>
      <td>70</td>
      <td>True</td>
      <td>No</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>64</td>
      <td>65</td>
      <td>True</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1</td>
      <td>72</td>
      <td>95</td>
      <td>False</td>
      <td>No</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1</td>
      <td>69</td>
      <td>70</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>9</th>
      <td>3</td>
      <td>75</td>
      <td>80</td>
      <td>False</td>
      <td>Yes</td>
    </tr>
  </tbody>
</table>
</div>



## 4. Splitting Data into Training and Test sets

We now need to randomly split the data into two sets. The first set will be sent to train the algorithm. The second to test the model to see if it can predict for us without being told the answer.


```python
# split the data into training and test data
train, test = cross_validation.train_test_split(golf_data,test_size=0.3, random_state=0)

# initialise Gaussian Naive Bayes
naive_b = GaussianNB()

# Use all columns apart from the Play column as feautures
train_features = train.ix[:,0:4]
# Use the play column as the label
train_label = train.iloc[:,4]

# Repeate above for test data
test_features = test.ix[:,0:4]
test_label = test.iloc[:,4]
```

## 5. Training and Prediction

Firstly we need to train our model. Here our features are sent along with the actual answer (survived column). The algorithm then uses this to build a model.

Our test data set will then be used to test the model. We give the model the test data (that it has never seen before) **without the answer(survived column)**. It will then try to predict based on the features whether or not that person would have survived.

We can see a sample of the test data along with the prediction and the overall accuracy below


```python
# Train the naive bayes model
naive_b.fit(train_features, train_label)

# build a dataframe to show the expected vs predicted values
test_data = pd.concat([test_features, test_label], axis=1)
test_data["prediction"] = naive_b.predict(test_features)

print test_data

# Use the score function and output the prediction accuracy
print "Naive Bayes Accuracy:", naive_b.score(test_features,test_label)
```

        Outlook  Temperature  Humidity   Wind Play prediction
    8         1           69        70  False  Yes        Yes
    6         2           64        65   True  Yes         No
    4         3           68        80  False  Yes        Yes
    11        2           72        90   True  Yes         No
    2         2           83        78  False  Yes        Yes
    Naive Bayes Accuracy: 0.6
    

`
