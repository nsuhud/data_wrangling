# Data wrangling

Data wrangling - also known as: data cleaning, data remediation, or data munging - is a set of processes aimed at transformaing raw data into more usable formats. The specific techniques used can vary depending on the particular data being used and the objectives of the project. 

Examples of data wrangling include consolidating multiple data sources into a single dataset for analysis, detecting gaps or missing data and resolving them, removing irrelevant or unnecessary data, and identify and addressing extreme outliers in the data to enable analysis.

## 1. Install and import required library
We are going to use [pandas](https://pandas.pydata.org/about/) library, an open-source software library created for the Python programming language that facilitates the manipulation and analysis of data. Pandas provides a range of data structures and functionalities for the processing of numerical tables and time series data.

```python
import pandas as pd
```

## 2. About the dataset
The dataset used in this exercise is the Automobile dataset available from [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/automobile). The information provided in this dataset is derived from the 1985 Ward's Automotive Yearbook, and the sources used to gather this data include:
1. The 1985 Model Import Car and Truck Specifications, 
2. Personal Auto Manuals from the Insurance Services Office
3. Insurance Collision Reports from the Insurance Institute for Highway Safety, in New York City and Washington, DC.

**The dataset can also be downloaded from the section above.**

## 3. Load the dataset with the related headers

### 3.1. The filepath
```python
filepath = "/insert-your-file-path-here/Automobile.csv"
```

### 3.2. The headers
```python
headers = ["symboling","normalized-losses","make","fuel-type","aspiration", "num-of-doors","body-style",
         "drive-wheels","engine-location","wheel-base", "length","width","height","curb-weight",
         "engine-type", "num-of-cylinders", "engine-size","fuel-system","bore","stroke",
         "compression-ratio","horsepower", "peak-rpm","city-mpg","highway-mpg","price"]
```

### 3.3. Load the dataset with the headers
```python
df = pd.read_csv(filename, names = headers)
```

Let's have a look at the first five rows of the dataset.
```python
df.head(5)
```

<img width="1247" alt="1" src="https://github.com/nsuhud/data_wrangling/assets/72127249/b9b9943f-2e58-4182-a625-859693d709b8">

The **question marks** in the dataset indicate missing values that can possibly affect further analysis. The following section provides a list of actions that can be taken to address these missing values.

## 4. Missing values

### 4.1. Identify the missing values

For ease of computational speed and convenience, all question marks "?" will be replaced with NaN (Not a Number), which is Python's default missing value marker. We will use the following function to carry out this replacement. 

The python's library that is going to be used is [numpy](https://numpy.org/), a library that allows for the handling of extensive, multi-dimensional arrays and matrices, as well as a vast assortment of advanced mathematical functions that work with these arrays and matrices.

```python
import numpy as np

df.replace("?", np.NaN, inplace = True)

df.head(5)
```

<img width="1243" alt="2" src="https://github.com/nsuhud/data_wrangling/assets/72127249/0c773557-600d-4ef0-963d-3dae7d079b23">

### 4.2. Evaluate the missing values
After running the codes above, all question marks should have been substituted with NaN. To verify this, we can apply one of the following functions:

<ol>
    <li><b>.isnull()</b></li>
    <li><b>.notnull()</b></li>
</ol>
    
The output of this function will be a boolean value of "True" or "False", where **True** indicates a missing value, and **False** shows a value present in the dataset.

```python
missing_data = df.isnull()
missing_data.head(5)
```

<img width="1101" alt="3" src="https://github.com/nsuhud/data_wrangling/assets/72127249/40ed0e41-0fd2-4879-9233-116b628cf7f0">

### 4.3. Count missing values
Using a Python for loop, we can easily count the missing values in each column. The "value_counts()" method within the loop tallies the occurrences of "True" values.

```python
for column in missing_data.columns.values.tolist():
    print(colum)
    print(missing_data[column].value_counts())
    print("")
```

<img width="425" alt="4" src="https://github.com/nsuhud/data_wrangling/assets/72127249/b859344c-09c6-4789-8ee0-19fe6533ff90">

Based on the summary, there are 205 data rows in each column, and seven columns have data that is incomplete.

1. normalized-losses: 41 missing data
2. num-of-doors: 2 missing data
3. bore: 4 missing data
4. stroke : 4 missing data
5. horsepower: 2 missing data
6. peak-rpm: 2 missing data
7. price: 4 missing data

### 4.4. Resolving missing values
Generally, there are two methods to resolve missing values:
<ol>
    <li>Drop the value/data<br>
        a. Drop the entire column<br>
        b. Drop the whole row
    </li>
    <li>Replace the value/data<br>
        a. Replace it by mean<br>
        b. Replace it by mode (frequent value/data)
        c. Replace it based on other functions
    </li>
</ol>

The option to drop the entire column can be chosen if most entries, values or data in the column are missing. The Automobile dataset does not contain any columns that are empty enough to justify opting for the choice of dropping the entire column. Therefore, we still have several available options to address the issue of missing values. We will apply different methods to various columns.

<b>Replace by mean:</b>
<ul>
    <li>"normalized-losses": 41 missing data, replace them with the mean</li>
    <li>"stroke": 4 missing data, replace them with the mean</li>
    <li>"bore": 4 missing data, replace them with the mean</li>
    <li>"horsepower": 2 missing data, replace them with the mean</li>
    <li>"peak-rpm": 2 missing data, replace them with the mean</li>
</ul>

<b>Replace by mode:</b>
<ul>
    <li>"num-of-doors": 2 missing data, replace them with the mode</li>
    <ul>Reason: With 84% of sedans featuring four doors, the four-door configuration is the most commong among sedans (mode), making it more likely to occur.</ul>
</ul>

<b>Drop the whole role:</b>
<ul>
    <li>"price": 4 missing data, delete the row</li>
    <ul>Reason: Price is what we want to predict. Any data entry without price data cannot be used for prediction; therefore, any row without price data is not useful to us.</ul>
</ul>

#### 4.4.1. Replace missing values with the mean value
<h4>Calculate the mean value for the "normalized-losses" column</h4>

We set axis = 0 to get the **mean** of all rows for each column (moving row-wise).

To ensure that all data in the column "normalized-losses' are of a float data type (retaining decimals and integers), we use the `astype()` method for conversion.

```python
avg_norm_loss = df["normalized-losses"].astype("float").mean(axis = 0)

print("Mean of normalized-losses: {:0.3f}".format(avg_norm_loss))
```

<img width="318" alt="5" src="https://github.com/nsuhud/data_wrangling/assets/72127249/777c9fd4-bdd9-4cbe-97e3-888a7c0163ff">

<h4>Replace "NaN" in the "normalized-losses" column with the mean value</h4>

```python
df["normalized-losses"].replace(np.NaN, avg_norm_loss, inplace  = True)
df["normalized-losses"]
```

<img width="472" alt="6" src="https://github.com/nsuhud/data_wrangling/assets/72127249/2aeaef17-8f17-4877-8da5-8b9fe2b0d4e4">

<h4>Calculate the mean value for the "bore" column</h4>

```python
avg_bore = df["bore"].astype("float").mean(axis = 0)

print("Mean of bore: {:0.3f}".format(avg_bore))
```

<img width="192" alt="7" src="https://github.com/nsuhud/data_wrangling/assets/72127249/a7c21079-e331-4323-8c75-162803b8d710">

<h4>Replace "NaN" in the "bore" column with the mean value</h4>

```python
df["bore"].replace(np.NaN, avg_bore, inplace = True)

df["bore"]
```

<img width="355" alt="8" src="https://github.com/nsuhud/data_wrangling/assets/72127249/4fbc53db-39d6-4022-960a-f71c205d435f">

<h4>Calculate the mean value for the "stroke" column</h4>

```python
avg_stroke = df["stroke"].astype("float").mean(axis = 0)

print("Mean of stroke: {:0.3f}".format(avg_stroke))
```

<img width="199" alt="9" src="https://github.com/nsuhud/data_wrangling/assets/72127249/a2bc8228-b231-4674-84d9-d6d5dc968940">

<h4>Replace "NaN" in the "stroke" column with the mean value</h4>

```python
df["stroke"].replace(np.NaN, avg_stroke, inplace = True)

df["stroke"]
```

<img width="373" alt="10" src="https://github.com/nsuhud/data_wrangling/assets/72127249/53445a73-2e0c-48e2-933d-16cc1c8e9770">

<h4>Calculate the mean value for the "horsepower" column</h4>

```python
avg_hp = df["horsepower"].astype("float").mean(axis = 0)

print("Mean of horsepower: {:0.3f}".format(avg_hp))
```

<img width="256" alt="11" src="https://github.com/nsuhud/data_wrangling/assets/72127249/8a970595-e43b-4ad0-8198-00bfad7f735c">

<h4>Replace "NaN" in the "horsepower" column with the mean value</h4>

```python
df["horsepower"].replace(np.NaN, avg_hp, inplace = True)

df["horsepower"]
```

<img width="421" alt="12" src="https://github.com/nsuhud/data_wrangling/assets/72127249/d5c0b5e6-4936-47fa-9ad8-3a3c69f1502a">

<h4>Calculate the mean value for the "peak-rpm" column</h4>

```python
avg_rpm = df["peak-rpm"].astype("float").mean(axis = 0)

print("Mean of peak-rpm: {:0.3f}".format(avg_rpm))
```

<img width="245" alt="13" src="https://github.com/nsuhud/data_wrangling/assets/72127249/94d16f79-1cc5-4756-a598-b5a5ec29d9b1">

<h4>Replace "NaN" in the "peak-rpm" column with the mean value</h4>

```python
df["peak-rpm"].replace(np.NaN, avg_rpm, inplace = True)

df["peak-rpm"]
```

<img width="393" alt="14" src="https://github.com/nsuhud/data_wrangling/assets/72127249/2bc49dba-bc68-49d6-9e03-6ef11474faf5">

#### 4.4.2. Replace missing values with the mode value (most frequent value)
<h4>Calculate the mode value for the "num-of-doors" column</h4>

```python
df["num-of-doors"].value_counts()
```

<img width="326" alt="15" src="https://github.com/nsuhud/data_wrangling/assets/72127249/313b4d1a-23ba-46d2-8eae-761037d59e61">

We can see that four doors are the most common type. To generate the most frequent value, we can also use the idxmax() method. The idxmax() method is used to find the index (row label) of the maximum value in a pandas Series or DataFrame.

```python
df["num-of-doors"].value_counts().idxmax()
```

<img width="86" alt="16" src="https://github.com/nsuhud/data_wrangling/assets/72127249/0368cae8-991c-4c5a-bc72-cf918af2526c">

<h4>Replace "NaN" in the "num-of-doors" column with the mode value</h4>

```python
df["num-of-doors"].replace(np.NaN, "four", inplace = True)

df["num-of-doors"]
```

<img width="444" alt="17" src="https://github.com/nsuhud/data_wrangling/assets/72127249/a7d66f66-ffbd-4185-a670-743692b1eebf">

#### 4.4.3. Drop all rows that do not have price data

```python
df.dropna(subset = ["price"], axis = 0, inplace = True)
```

Reset the index after we drop the rows and check whether it has been properly reset:

```python
df.reset_index(drop = True, inplace = True)

df.head()
```

<img width="1144" alt="18" src="https://github.com/nsuhud/data_wrangling/assets/72127249/605fc6d2-66fc-4db3-a96c-57949d80d774">

## 5. Correcting the data format in the dataset
Let's first check the data types in the dataset using the following method:

```python
df.dtypes
```

<img width="301" alt="19" src="https://github.com/nsuhud/data_wrangling/assets/72127249/7e54cd35-77c0-4649-81cc-bde008aa4bab">

Above, certain columns have incorrect data types. Numerical variables should be 'float' or 'int', while those with string values, such as categories, should be 'object'. 

For example, the parameter 'bore' and 'stroke', 'price', and 'peak-rpm' should be numeric types or 'float', but they are currently 'object'. While 'normalized-losses' should be 'int'.

To resolve this, we'll use the astype() method to convert data types as needed.

```python
df[["bore", "stroke", "price", "peak-rpm"]] = df[["bore", "stroke", "price", "peak-rpm"]].astype("float")

df["normalized-losses"] = df["normalized-losses"].astype("int")
```

Let's list the columns after performing the conversion by running this command:

```python
df[["bore", "stroke", "price", "peak-rpm", "normalized-losses"]].dtypes
```

<img width="288" alt="20" src="https://github.com/nsuhud/data_wrangling/assets/72127249/084df7da-4952-4190-9977-febaef3c6390">

We now have the cleaned dataset, with no missing values, and all data in the right format.

## 6. Data Standardization

Data is typically gathered from various agencies in diverse formats. Data standardization also refers to a specific form of data normalization involving the subtraction of the mean and division by the standard deviation.

In our context of data preparation, data standardization is the process of converting data into a uniform format, enabling meaningful comparisons for researchers.

In our dataset, the "city-mpg" and "highway-mpg" fuel consumption columns are in mpg (miles per gallon). Suppose we're creating an application for a country that uses L/100km as the standard for fuel consumption. In that case, we'll need to perform a data transformation to convert mpg to L/100km.

The formula for unit conversion is:

L/100km = 235 / mpg

<h4>Convert the "city-mpg" to "city-L/100km"</h4>

```python
df["city-L/100km"] = 235/df["city-mpg"]
df.head()
```

<img width="1174" alt="21" src="https://github.com/nsuhud/data_wrangling/assets/72127249/da10555f-2df7-4f96-bd1d-990bc525d143">

<h4>Convert the "highway-mpg" to "highway-L/100km"</h4>

```python
df["highway-L/100km"] = 235/df["highway-mpg"]
df.head()
```

<img width="1221" alt="22" src="https://github.com/nsuhud/data_wrangling/assets/72127249/bca12cba-5120-4d70-81f0-42a922501ff3">

## 7. Data Normalization

Data normalization is a key step in preparing data, ensuring variable values are in a consistent range. It involves techniques like scaling variables to achieve goals such as setting the average to 0, or ensuring a variance of 1, or limiting values between 0 and 1.

To illustrate the concept of normalization, let's consider the scenario where we aim to normalize the "length", "width", and "height" columns.

We will se the formula (original value)/(maximum value) to replace the original values and effectively scaling them for normalization.

```python
df["length"] = df["length"]/df["length"].max()
df["width"] = df["width"]/df["width"].max()
df["height"] = df["height"]/df["height"].max()

```

Let's take a look at the normalized columns:
```python
df[["length", "width", "height"]].head()
```

<img width="297" alt="23" src="https://github.com/nsuhud/data_wrangling/assets/72127249/8ad8b320-36c9-4d2c-a6ae-38e959075298">

## 8. Binning

Binning involves converting continuous numerical variables into distinct categorical "bins" for more straightforward group analysis.

Let's take the "horsepower" variable in the dataset, ranging from 48 to 288 with 59 unique values. If the goal is to compare price differences among cars with high, medium and low horsepower (3 categories), we can simplify the analysis by grouping them into three clear categories or bins.

To achieve this, we'll use the `cut` function in pandas to segment the horsepower column into 3 bins.

First, let's ensure the data is in the correct format.

```python
df["horsepower"] = df["horsepower"].astype(int, copy = True)
```

Plot the histogram of horsepower to observe the distribution of horsepower.
```python
%matplotlib inline
import matplotlib as plt
from matplotlib import pyplot
plt.pyplot.hist(df["horsepower"])
```

Set x and y labels, as well as the plot title.
```python
plt.pyplot.xlabel("horsepower")
plt.pyplot.ylabel("count")
plt.pyplot.title("Horsepower Bins")
```

<img width="658" alt="24" src="https://github.com/nsuhud/data_wrangling/assets/72127249/d8bc64cb-a431-4fcb-b870-ab88054a68d6">

Here, we would like to create three equal-sized bins to analyse the distribution of horsepower in a dataset. We will use NumPy's linspace function, setting the start and end values to include the dataset's minimum and maximum values.

For our specific case of building three bins, we need four dividers, so we set `numbers_generated` to 4.

The bin array is then constructed, covering the range from the **minimum** to the **maximum** horsepower values. Each value in the array marks the boundaries between bins, enabling a thorough analysis of horsepower distribution.

```python
bins = np.linspace(min(df["horsepower"]), max(df["horsepower"]), 4)
bins
```

<img width="567" alt="25" src="https://github.com/nsuhud/data_wrangling/assets/72127249/9eadeb41-4a10-42f0-9212-69ae0e37b9d2">

We assign group names and use the `cut` function to categorize each df["horsepower"] value.
```python
group_names = ["Low", "Medium", "High"]

df["horsepower-binned"] = pd.cut(df["horsepower"], bins, labels = group_names, include_lowest = True)

df[["horsepower", "horsepower-binned"]].head(10)
```

<img width="308" alt="26" src="https://github.com/nsuhud/data_wrangling/assets/72127249/4a005982-0689-4a2e-b16c-1567324fef9b">

Check the number of vehicles in each bin.
```python
df["horsepower-binned"].value_counts()
```

<img width="352" alt="27" src="https://github.com/nsuhud/data_wrangling/assets/72127249/262565be-dc8f-42b9-887e-375fa7ca3b0a">

Now, we will create a visualization for the distribution of each bin.
```python
%matplotlib inline
pyplot.bar(group_names, df["horsepower-binned"].value_counts())

plt.pyplot.xlabel("horsepower")
plt.pyplot.ylabel("count")
plt.pyplot.title("Horsepower bins")
```

<img width="657" alt="28" src="https://github.com/nsuhud/data_wrangling/assets/72127249/003d88ba-4700-424e-9bbb-b345f9696415">

Let's use a histogram to visualize the distribution of the bins we generated earlier.
```python
%matplotlib inline

pyplot.hist(df["horsepower"], bins = 3)

plt.pyplot.xlabel("horsepower")
plt.pyplot.ylabel("count")
plt.pyplot.title("Horsepower bins")
```

<img width="673" alt="29" src="https://github.com/nsuhud/data_wrangling/assets/72127249/5c21330e-7741-4637-b317-630aac2580cc">

## 9. Dummy Variable

A dummy variable, also known as indicator variable, is a numerical representation used to categorise different groups. The term "dummy" is used because the numerical values themselves lack inherent meaning.

The purpose of using indicator variables is to facilitate the use of categorical variables for regression analysis. For our dataset, we will generate dummy variables for two columns with categorical data: fuel-type and aspiration. 

We will use the get_dummies method from the Pandas library to generate dummy variables and assign them to different categories of fuel types and aspiration. Let's first create dummy variables for fuel types.

```python
#1. Generate the dummy variables and assign them to dataframe "dummy_variable_1":
dummy_variable_1 = pd.get_dummies(df["fuel-type"])
dummy_variable_1.head()
```

<img width="154" alt="30" src="https://github.com/nsuhud/data_wrangling/assets/72127249/2e5de929-0777-4820-864d-3b38c9c35a05">

```python
#2. Re-name the columns for clarity:
dummy_variable_1.rename(columns = {"gas":"fuel-type-gas", "diesel":"fuel-type-diesel"}, inplace = True)
dummy_variable_1.head()
```

<img width="298" alt="31" src="https://github.com/nsuhud/data_wrangling/assets/72127249/64199282-d5f6-42b2-86d6-c88d00d46404">

```python
#3. Merge data frame "df" and "dummy_variable_1" by setting the axis to 1 to combine horizontally along the x axis:
df = pd.concat([df, dummy_variable_1], axis = 1)

#4. Drop the original column "fuel-type" from df:
df.drop("fuel-type", axis = 1, inplace = True)

#5. Check the final result:
df.head(5)
```

<img width="1193" alt="32" src="https://github.com/nsuhud/data_wrangling/assets/72127249/fe0028f9-8c00-490c-acb0-2b0ae2da8e15">

Now, let's create dummy variables for aspiration.

```python
#1. Generate the dummy variables and assign them to dataframe "dummy_variable_2":
dummy_variable_2 = pd.get_dummies(df["aspiration"])
dummy_variable_2.head()
```

<img width="154" alt="33" src="https://github.com/nsuhud/data_wrangling/assets/72127249/bada998f-3103-485e-bcba-2739452eb3a2">

```python
#2. Re-name the columns for clarity:
dummy_variable_2.rename(columns = {"std":"aspiration-std", "turbo":"aspiration-turbo"}, inplace = True)
dummy_variable_2.head()
```

<img width="293" alt="34" src="https://github.com/nsuhud/data_wrangling/assets/72127249/fc3ef271-4520-49ed-a748-25cf65478da5">

```python
#3. Merge data frame "df" and "dummy_variable_2" by setting the axis to 1 to combine horizontally along the x axis:
df = pd.concat([df, dummy_variable_2], axis = 1)

#4. Drop the original "aspiration" column from df:
df.drop("aspiration", axis = 1, inplace = True)

#5. Check the final result:
df.head(5)
```

<img width="1261" alt="35" src="https://github.com/nsuhud/data_wrangling/assets/72127249/7371ecdb-32a1-48c3-aefc-f1dca4dcf0ac">

<p><h4>Now our dataset is ready for further analysis!</h4></p>
