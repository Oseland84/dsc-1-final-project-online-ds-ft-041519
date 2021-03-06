
## Final Project Submission


* Nick Oseland 
* full time
* Scheduled project review date/time: 
* Instructor name: Rafael Carrasco 
* Blog post URL: https://oseland84.github.io/exploratory_data_analysis


### Introduction
In this notebook, we will be taking an in-deth look at a dataset of homes sold between May 2014 and May 2015 in King's County in Washington (State). The objective is to identify trends and gain insight about the data set. We would also like to ultimetly define what attributes of a home have the strongest impact on what the home will go on to sell for. 


* [Obtaining Data](#Obtain)
* [Scrubbing/Cleaning Data](#Scrubbing)
    * Handling Null Values
* [EDA - Exploratory Data Analysis](#EDA)
    * Questions should go here
    * Visualizations should go here
* [Modeling](#Modeling)
* [Investigate Models](#Investigate)

### The OSEMN framework
   **Data science Process**
#### 1. Obtain Data
The very first step of a data science project is straightforward. We obtain the data that we need from available data sources. In this step, you will need to query databases, using technical skills like MySQL to process the data. You may also receive data in file formats like Microsoft Excel. If you are using Python or R, they have specific packages that can read data from these data sources directly into your data science programs.
#### 2. Scrub Data
After obtaining data, the next immediate thing to do is scrubbing data. This process is for us to “clean” and to filter the data. Remember the “garbage in, garbage out” philosophy, if the data is unfiltered and irrelevant, the results of the analysis will not mean anything.
#### 3. Explore Data
Once your data is ready to be used, and right before you jump into AI and Machine Learning, you will have to examine the data. to do this, you must explore the data. You must inspect the data and its properties. Different data types like numerical data, categorical data, ordinal and nominal data etc. require different treatments.
#### 4. Model Data
This is where things start to get interesting, or as some put it: “where the magic happens”. In short, we use regression and predictions for forecasting future values, and classification to identify, and clustering to group values.
#### 5. Interpreting Data
The final and most crucial stage. The predictive power of a model lies in its ability to generalise. Interpreting data refers to the presentation of your data to a non-technical layman. We deliver the results in to answer the business questions we asked when we first started the project, together with the actionable insights that we found through the data science process.

In the future, I will import only the necessary libraries. While I'm still so new to this, I wanted to import everything I've seen thus far, so as not to limit myself. 


```python
# Importing all librairies that I may need to use.
import numpy as np
import pandas as pd

import statsmodels.formula.api as smf
import statsmodels.api as sm
import statsmodels.stats as sts
import scipy.stats as stats

from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.model_selection import cross_val_score
from sklearn import metrics
from sklearn.feature_selection import RFE
from sklearn.metrics import r2_score

%matplotlib inline
cross_val_score
from sklearn.metrics import mean_squared_error
import matplotlib.pyplot as plt
import seaborn as sns
from seaborn import lmplot
```

## Obtain


```python
#Taking a look at first 5 rows from data set
df = pd.read_csv("kc_house_data.csv")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>price</th>
      <th>bedrooms</th>
      <th>bathrooms</th>
      <th>sqft_living</th>
      <th>sqft_lot</th>
      <th>floors</th>
      <th>waterfront</th>
      <th>view</th>
      <th>...</th>
      <th>grade</th>
      <th>sqft_above</th>
      <th>sqft_basement</th>
      <th>yr_built</th>
      <th>yr_renovated</th>
      <th>zipcode</th>
      <th>lat</th>
      <th>long</th>
      <th>sqft_living15</th>
      <th>sqft_lot15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7129300520</td>
      <td>10/13/2014</td>
      <td>221900.0</td>
      <td>3</td>
      <td>1.00</td>
      <td>1180</td>
      <td>5650</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1180</td>
      <td>0.0</td>
      <td>1955</td>
      <td>0.0</td>
      <td>98178</td>
      <td>47.5112</td>
      <td>-122.257</td>
      <td>1340</td>
      <td>5650</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6414100192</td>
      <td>12/9/2014</td>
      <td>538000.0</td>
      <td>3</td>
      <td>2.25</td>
      <td>2570</td>
      <td>7242</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>2170</td>
      <td>400.0</td>
      <td>1951</td>
      <td>1991.0</td>
      <td>98125</td>
      <td>47.7210</td>
      <td>-122.319</td>
      <td>1690</td>
      <td>7639</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5631500400</td>
      <td>2/25/2015</td>
      <td>180000.0</td>
      <td>2</td>
      <td>1.00</td>
      <td>770</td>
      <td>10000</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>6</td>
      <td>770</td>
      <td>0.0</td>
      <td>1933</td>
      <td>NaN</td>
      <td>98028</td>
      <td>47.7379</td>
      <td>-122.233</td>
      <td>2720</td>
      <td>8062</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2487200875</td>
      <td>12/9/2014</td>
      <td>604000.0</td>
      <td>4</td>
      <td>3.00</td>
      <td>1960</td>
      <td>5000</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1050</td>
      <td>910.0</td>
      <td>1965</td>
      <td>0.0</td>
      <td>98136</td>
      <td>47.5208</td>
      <td>-122.393</td>
      <td>1360</td>
      <td>5000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1954400510</td>
      <td>2/18/2015</td>
      <td>510000.0</td>
      <td>3</td>
      <td>2.00</td>
      <td>1680</td>
      <td>8080</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>8</td>
      <td>1680</td>
      <td>0.0</td>
      <td>1987</td>
      <td>0.0</td>
      <td>98074</td>
      <td>47.6168</td>
      <td>-122.045</td>
      <td>1800</td>
      <td>7503</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



[Back to Introduction](#Introduction)

# Scrubbing
At this stage, I'm trying to get a feel for the data set.

## Column Names and descriptions for Kings County Data Set
* **id** - unique identified for a house
* **dateDate** - house was sold
* **pricePrice** -  is prediction target
* **bedroomsNumber** -  of Bedrooms/House
* **bathroomsNumber** -  of bathrooms/bedrooms
* **sqft_livingsquare** -  footage of the home
* **sqft_lotsquare** -  footage of the lot
* **floorsTotal** -  floors (levels) in house
* **waterfront** - House which has a view to a waterfront
* **view** - Has been viewed
* **condition** - How good the condition is ( Overall )
* **grade** - overall grade given to the housing unit, based on King County grading system
* **sqft_above** - square footage of house apart from basement
* **sqft_basement** - square footage of the basement
* **yr_built** - Built Year
* **yr_renovated** - Year when house was renovated
* **zipcode** - zip
* **lat** - Latitude coordinate
* **long** - Longitude coordinate
* **sqft_living15** - The square footage of interior housing living space for the nearest 15 neighbors
* **sqft_lot15** - The square footage of the land lots of the nearest 15 neighbors



```python
#rows and columns from data set (DF)
df.shape
```




    (21597, 21)




```python
#randomly displays 20 rows within dataset, ran this several times to get a better look.
df.sample(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>price</th>
      <th>bedrooms</th>
      <th>bathrooms</th>
      <th>sqft_living</th>
      <th>sqft_lot</th>
      <th>floors</th>
      <th>waterfront</th>
      <th>view</th>
      <th>...</th>
      <th>grade</th>
      <th>sqft_above</th>
      <th>sqft_basement</th>
      <th>yr_built</th>
      <th>yr_renovated</th>
      <th>zipcode</th>
      <th>lat</th>
      <th>long</th>
      <th>sqft_living15</th>
      <th>sqft_lot15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4619</th>
      <td>2581900165</td>
      <td>10/21/2014</td>
      <td>1130000.0</td>
      <td>4</td>
      <td>3.50</td>
      <td>4300</td>
      <td>8406</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>11</td>
      <td>3580</td>
      <td>720.0</td>
      <td>1987</td>
      <td>0.0</td>
      <td>98040</td>
      <td>47.5396</td>
      <td>-122.214</td>
      <td>2770</td>
      <td>10006</td>
    </tr>
    <tr>
      <th>19146</th>
      <td>6072500490</td>
      <td>8/1/2014</td>
      <td>423800.0</td>
      <td>3</td>
      <td>2.50</td>
      <td>1940</td>
      <td>7415</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>8</td>
      <td>1940</td>
      <td>0.0</td>
      <td>1965</td>
      <td>0.0</td>
      <td>98006</td>
      <td>47.5420</td>
      <td>-122.176</td>
      <td>1940</td>
      <td>8425</td>
    </tr>
    <tr>
      <th>2627</th>
      <td>8825900070</td>
      <td>8/18/2014</td>
      <td>705000.0</td>
      <td>6</td>
      <td>2.00</td>
      <td>2570</td>
      <td>4240</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1970</td>
      <td>600.0</td>
      <td>1911</td>
      <td>0.0</td>
      <td>98115</td>
      <td>47.6754</td>
      <td>-122.307</td>
      <td>2030</td>
      <td>4240</td>
    </tr>
    <tr>
      <th>3758</th>
      <td>2523089097</td>
      <td>10/29/2014</td>
      <td>524500.0</td>
      <td>3</td>
      <td>1.50</td>
      <td>3430</td>
      <td>264844</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>7</td>
      <td>2230</td>
      <td>1200.0</td>
      <td>1988</td>
      <td>0.0</td>
      <td>98045</td>
      <td>47.4476</td>
      <td>-121.723</td>
      <td>1660</td>
      <td>145926</td>
    </tr>
    <tr>
      <th>4170</th>
      <td>2461900945</td>
      <td>4/21/2015</td>
      <td>438000.0</td>
      <td>5</td>
      <td>1.00</td>
      <td>1950</td>
      <td>6250</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1450</td>
      <td>?</td>
      <td>1917</td>
      <td>0.0</td>
      <td>98136</td>
      <td>47.5511</td>
      <td>-122.386</td>
      <td>1950</td>
      <td>6250</td>
    </tr>
    <tr>
      <th>15221</th>
      <td>809003105</td>
      <td>4/8/2015</td>
      <td>935000.0</td>
      <td>3</td>
      <td>2.00</td>
      <td>1720</td>
      <td>2000</td>
      <td>1.5</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>...</td>
      <td>8</td>
      <td>1060</td>
      <td>660.0</td>
      <td>1910</td>
      <td>2000.0</td>
      <td>98109</td>
      <td>47.6384</td>
      <td>-122.350</td>
      <td>1590</td>
      <td>4000</td>
    </tr>
    <tr>
      <th>14612</th>
      <td>179001046</td>
      <td>5/8/2014</td>
      <td>229000.0</td>
      <td>3</td>
      <td>2.50</td>
      <td>1190</td>
      <td>3000</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1190</td>
      <td>0.0</td>
      <td>2002</td>
      <td>NaN</td>
      <td>98178</td>
      <td>47.4933</td>
      <td>-122.275</td>
      <td>1190</td>
      <td>3000</td>
    </tr>
    <tr>
      <th>6484</th>
      <td>1843130980</td>
      <td>5/6/2014</td>
      <td>284000.0</td>
      <td>4</td>
      <td>2.50</td>
      <td>2000</td>
      <td>5390</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>2000</td>
      <td>0.0</td>
      <td>2003</td>
      <td>0.0</td>
      <td>98042</td>
      <td>47.3732</td>
      <td>-122.129</td>
      <td>2330</td>
      <td>5390</td>
    </tr>
    <tr>
      <th>18610</th>
      <td>2923501130</td>
      <td>7/22/2014</td>
      <td>588000.0</td>
      <td>4</td>
      <td>2.25</td>
      <td>2580</td>
      <td>7344</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>...</td>
      <td>8</td>
      <td>2580</td>
      <td>0.0</td>
      <td>1977</td>
      <td>0.0</td>
      <td>98027</td>
      <td>47.5647</td>
      <td>-122.090</td>
      <td>2390</td>
      <td>7507</td>
    </tr>
    <tr>
      <th>19639</th>
      <td>723049434</td>
      <td>4/8/2015</td>
      <td>369950.0</td>
      <td>3</td>
      <td>2.50</td>
      <td>1930</td>
      <td>8254</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1930</td>
      <td>0.0</td>
      <td>2014</td>
      <td>0.0</td>
      <td>98146</td>
      <td>47.4973</td>
      <td>-122.346</td>
      <td>1540</td>
      <td>8849</td>
    </tr>
    <tr>
      <th>12033</th>
      <td>7784400070</td>
      <td>7/22/2014</td>
      <td>585000.0</td>
      <td>3</td>
      <td>1.75</td>
      <td>1740</td>
      <td>9500</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>8</td>
      <td>1150</td>
      <td>590.0</td>
      <td>1958</td>
      <td>0.0</td>
      <td>98146</td>
      <td>47.4919</td>
      <td>-122.365</td>
      <td>2110</td>
      <td>9450</td>
    </tr>
    <tr>
      <th>7808</th>
      <td>7987400475</td>
      <td>4/17/2015</td>
      <td>745000.0</td>
      <td>5</td>
      <td>3.00</td>
      <td>2400</td>
      <td>10126</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>8</td>
      <td>2400</td>
      <td>0.0</td>
      <td>1981</td>
      <td>0.0</td>
      <td>98126</td>
      <td>47.5726</td>
      <td>-122.373</td>
      <td>2250</td>
      <td>3946</td>
    </tr>
    <tr>
      <th>19066</th>
      <td>1825049013</td>
      <td>2/13/2015</td>
      <td>560000.0</td>
      <td>4</td>
      <td>2.00</td>
      <td>1380</td>
      <td>4048</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1380</td>
      <td>0.0</td>
      <td>1906</td>
      <td>0.0</td>
      <td>98103</td>
      <td>47.6583</td>
      <td>-122.344</td>
      <td>1440</td>
      <td>3956</td>
    </tr>
    <tr>
      <th>15214</th>
      <td>8644500010</td>
      <td>3/20/2015</td>
      <td>715000.0</td>
      <td>3</td>
      <td>1.75</td>
      <td>1650</td>
      <td>7276</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>7</td>
      <td>1150</td>
      <td>500.0</td>
      <td>1928</td>
      <td>0.0</td>
      <td>98117</td>
      <td>47.6989</td>
      <td>-122.399</td>
      <td>2300</td>
      <td>8088</td>
    </tr>
    <tr>
      <th>18537</th>
      <td>2568200740</td>
      <td>8/11/2014</td>
      <td>720000.0</td>
      <td>5</td>
      <td>2.75</td>
      <td>2860</td>
      <td>5379</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>9</td>
      <td>2860</td>
      <td>0.0</td>
      <td>2005</td>
      <td>0.0</td>
      <td>98052</td>
      <td>47.7082</td>
      <td>-122.104</td>
      <td>2980</td>
      <td>6018</td>
    </tr>
    <tr>
      <th>18964</th>
      <td>9346700150</td>
      <td>7/2/2014</td>
      <td>552000.0</td>
      <td>3</td>
      <td>2.50</td>
      <td>1840</td>
      <td>9900</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>9</td>
      <td>1840</td>
      <td>0.0</td>
      <td>1978</td>
      <td>NaN</td>
      <td>98007</td>
      <td>47.6131</td>
      <td>-122.151</td>
      <td>2730</td>
      <td>9900</td>
    </tr>
    <tr>
      <th>1700</th>
      <td>8651402920</td>
      <td>5/5/2014</td>
      <td>219900.0</td>
      <td>4</td>
      <td>1.50</td>
      <td>1120</td>
      <td>5427</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>6</td>
      <td>1120</td>
      <td>0.0</td>
      <td>1969</td>
      <td>NaN</td>
      <td>98042</td>
      <td>47.3628</td>
      <td>-122.087</td>
      <td>1150</td>
      <td>5304</td>
    </tr>
    <tr>
      <th>20238</th>
      <td>1332700020</td>
      <td>1/16/2015</td>
      <td>278000.0</td>
      <td>2</td>
      <td>2.25</td>
      <td>1610</td>
      <td>1968</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1610</td>
      <td>0.0</td>
      <td>1979</td>
      <td>0.0</td>
      <td>98056</td>
      <td>47.5184</td>
      <td>-122.196</td>
      <td>1950</td>
      <td>1968</td>
    </tr>
    <tr>
      <th>16900</th>
      <td>1324079007</td>
      <td>11/10/2014</td>
      <td>425000.0</td>
      <td>3</td>
      <td>1.75</td>
      <td>1610</td>
      <td>144619</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>7</td>
      <td>1610</td>
      <td>0.0</td>
      <td>1977</td>
      <td>0.0</td>
      <td>98024</td>
      <td>47.5659</td>
      <td>-121.863</td>
      <td>2220</td>
      <td>144619</td>
    </tr>
    <tr>
      <th>7997</th>
      <td>9558020610</td>
      <td>5/12/2014</td>
      <td>335000.0</td>
      <td>3</td>
      <td>2.50</td>
      <td>1940</td>
      <td>4927</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>...</td>
      <td>8</td>
      <td>1940</td>
      <td>0.0</td>
      <td>2004</td>
      <td>0.0</td>
      <td>98058</td>
      <td>47.4479</td>
      <td>-122.120</td>
      <td>2070</td>
      <td>4892</td>
    </tr>
  </tbody>
</table>
<p>20 rows × 21 columns</p>
</div>




```python
#.describe()basic statistical details like percentile, mean, std etc. of the data
np.round(df.describe())
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>price</th>
      <th>bedrooms</th>
      <th>bathrooms</th>
      <th>sqft_living</th>
      <th>sqft_lot</th>
      <th>floors</th>
      <th>waterfront</th>
      <th>view</th>
      <th>condition</th>
      <th>grade</th>
      <th>sqft_above</th>
      <th>yr_built</th>
      <th>yr_renovated</th>
      <th>zipcode</th>
      <th>lat</th>
      <th>long</th>
      <th>sqft_living15</th>
      <th>sqft_lot15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2.159700e+04</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>19221.0</td>
      <td>21534.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>17755.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
      <td>21597.0</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4.580474e+09</td>
      <td>540297.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>2080.0</td>
      <td>15099.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>1789.0</td>
      <td>1971.0</td>
      <td>84.0</td>
      <td>98078.0</td>
      <td>48.0</td>
      <td>-122.0</td>
      <td>1987.0</td>
      <td>12758.0</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2.876736e+09</td>
      <td>367368.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>918.0</td>
      <td>41413.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>828.0</td>
      <td>29.0</td>
      <td>400.0</td>
      <td>54.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>685.0</td>
      <td>27274.0</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000102e+06</td>
      <td>78000.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>370.0</td>
      <td>520.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>370.0</td>
      <td>1900.0</td>
      <td>0.0</td>
      <td>98001.0</td>
      <td>47.0</td>
      <td>-123.0</td>
      <td>399.0</td>
      <td>651.0</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.123049e+09</td>
      <td>322000.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1430.0</td>
      <td>5040.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>1190.0</td>
      <td>1951.0</td>
      <td>0.0</td>
      <td>98033.0</td>
      <td>47.0</td>
      <td>-122.0</td>
      <td>1490.0</td>
      <td>5100.0</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.904930e+09</td>
      <td>450000.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1910.0</td>
      <td>7618.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>1560.0</td>
      <td>1975.0</td>
      <td>0.0</td>
      <td>98065.0</td>
      <td>48.0</td>
      <td>-122.0</td>
      <td>1840.0</td>
      <td>7620.0</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>7.308900e+09</td>
      <td>645000.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>2550.0</td>
      <td>10685.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>8.0</td>
      <td>2210.0</td>
      <td>1997.0</td>
      <td>0.0</td>
      <td>98118.0</td>
      <td>48.0</td>
      <td>-122.0</td>
      <td>2360.0</td>
      <td>10083.0</td>
    </tr>
    <tr>
      <th>max</th>
      <td>9.900000e+09</td>
      <td>7700000.0</td>
      <td>33.0</td>
      <td>8.0</td>
      <td>13540.0</td>
      <td>1651359.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>13.0</td>
      <td>9410.0</td>
      <td>2015.0</td>
      <td>2015.0</td>
      <td>98199.0</td>
      <td>48.0</td>
      <td>-121.0</td>
      <td>6210.0</td>
      <td>871200.0</td>
    </tr>
  </tbody>
</table>
</div>



## Handling Null Values
At this point, I'm focused on which columns have null values, incorrect data type, or too many zeros.


```python
# check for missing (NaN) values, I prefer .sum() over .any()
# in this instance as it shows me how many for each column
df.isnull().sum() 
```




    id                  0
    date                0
    price               0
    bedrooms            0
    bathrooms           0
    sqft_living         0
    sqft_lot            0
    floors              0
    waterfront       2376
    view               63
    condition           0
    grade               0
    sqft_above          0
    sqft_basement       0
    yr_built            0
    yr_renovated     3842
    zipcode             0
    lat                 0
    long                0
    sqft_living15       0
    sqft_lot15          0
    dtype: int64



So we have some missing values.
waterfront = 2376, 
view = 63, 
yr_renovated = 3,842.


```python
#.info() function is used to get a concise summary of the dataframe.
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 21597 entries, 0 to 21596
    Data columns (total 21 columns):
    id               21597 non-null int64
    date             21597 non-null object
    price            21597 non-null float64
    bedrooms         21597 non-null int64
    bathrooms        21597 non-null float64
    sqft_living      21597 non-null int64
    sqft_lot         21597 non-null int64
    floors           21597 non-null float64
    waterfront       19221 non-null float64
    view             21534 non-null float64
    condition        21597 non-null int64
    grade            21597 non-null int64
    sqft_above       21597 non-null int64
    sqft_basement    21597 non-null object
    yr_built         21597 non-null int64
    yr_renovated     17755 non-null float64
    zipcode          21597 non-null int64
    lat              21597 non-null float64
    long             21597 non-null float64
    sqft_living15    21597 non-null int64
    sqft_lot15       21597 non-null int64
    dtypes: float64(8), int64(11), object(2)
    memory usage: 3.5+ MB



```python
#feel pretty safe in dropping ID at this point
df = df.drop(['id'], axis=1)
```


```python
#view looks to be the correct data type, so we will just deal with missing values.
df['view'].value_counts()
```




    0.0    19422
    2.0      957
    3.0      508
    1.0      330
    4.0      317
    Name: view, dtype: int64




```python
#replace NaN with 0
df['view'].fillna(0,inplace=True)
```

Year renovated is one that really threw me for a moment. While I am fairly confident that renovations will have a direct impact on housing prices. I thought it was also too ambiguous to really try to account for. Though part of my reasoning had to do with the fact that it was missing values and I didn’t want to deal with it (as well as several others we won't get into). Suffice it to say, this truly is an iterative process. I will try to keep my thought process easy to follow from beginning to end and not lead anyone into the quagmire that was the actual process. Just know that my approach and process was constantly shifting, as I better understood the data I was working with, or just discovering better ways of cleaning data while I was trying to find answers. 

In the end, I decided that it made the most sense to me as a simple yes or no.... i.e., True or False (bool). 


```python
# replacing NaN values with a 0
df['yr_renovated'].fillna(0, inplace=True)

#make sure it worked
df['yr_renovated'].isnull().sum()
```




    0



Awsome, moving on.

## Data Conditioning


```python
#change to boolean data type
df['yr_renovated'] = df['yr_renovated'].astype(bool)

#we should probably rename it to better represent this change
df['was_renovated'] = df['yr_renovated']
```


```python
#waterfront also makes more sense as a boolean, so...
df['waterfront'] = df['waterfront'].astype('bool')
```

 While looking for something else entirely, I discovered why I had been having issues with sq. ft. basement. While trying to convert its data type, I kept getting an error message I could not decipher. I'll spare you the bulk of it, but it ended with '?' which I could not figure out. I even pestered my instructor about it (to no avail), who told me I needed to figure it out...so I knew I was missing something. That’s when I saw '?' was an actual input found in sq. ft. basement column. I also got the impression that having sq. ft. basement sq. ft. above, s sq. ft. living could get a bit "muddy". After some trial and error, I came upon someone who had a similar problem, and came up with an elegant solution I will try to re-create with this data set.


```python
df['sqft_basement'].value_counts().head()
```




    0.0      12826
    ?          454
    600.0      217
    500.0      209
    700.0      208
    Name: sqft_basement, dtype: int64




```python
#locate rows containing ? DataFrame. loc. Access a group of rows and columns by label(s) 
#or a boolean array. .loc[] is primarily label based, but may also be used with a boolean array.
df.loc[df['sqft_basement'] == '?',['sqft_living','sqft_above']]

#locate rows with a different value for sqft_above and sqft_living
df.loc[(df["sqft_basement"] == '?') & (df["sqft_above"]!=df['sqft_living'])]

#Replace ? with difference between sqft_above and sqft_living.
#The abs() method returns the absolute value of the given number.
df['sqft_basement'].replace(to_replace='?', value = abs(df['sqft_above']-df['sqft_living']), inplace=True)

#double check that everything was replaced correctly
df['sqft_basement'].describe()
```




    count     21597
    unique      397
    top         0.0
    freq      12826
    Name: sqft_basement, dtype: object



If I am reading this correctly, then it means there 12,826 houses with out basements. We should probably just convert this to a boolean, it does or does not have a basement. It also looks like sqft_living includes the sq. footage of the basement. This makes the data misleading, or corrupted. 


```python
#pandas.to_numeric Convert argument to a numeric type
df['sqft_basement'] = pd.to_numeric(df['sqft_basement'])

#convert column into boolean
df['sqft_basement'] = df['sqft_basement'].astype(bool)

#create column name to more accurately represent data
df['has_basement'] = df['sqft_basement']

#drop values for sqft_basement, sqft_living, and yr_renovated
df = df.drop(["sqft_basement", "sqft_living","yr_renovated"], axis=1)
```


```python
#making sure the columns were dropped.
df.shape
```




    (21597, 19)



I feel it is worth mentioning that at this point I stopped the kernal and cleared the outputs. I had changed my mind, shifted most of the cells up and down, and generally completely lost track of how the kernals were executing (and started kicking errors on cells I thought I had already moved past). I worked back through the cells in order, and mercifully, everything worked (so far).

### Converting 'date' to a datetime feature
#this also took a few trials


```python
#Date converted into datetime
df['date'] = pd.to_datetime(df['date'])

#double check everything looks right
df['date'].head()

# query the data type for date column
#type(df['date'][0])
```




    0   2014-10-13
    1   2014-12-09
    2   2015-02-25
    3   2014-12-09
    4   2015-02-18
    Name: date, dtype: datetime64[ns]



[Back to Introduction](#Introduction)

# EDA

#### At this stage, I have worked through, and cleaned the data fairly well (and changed my mind about what attributes to focus on several times over).
It is time to start asking our data some questions. Not only we are tasked with asking, and hopefully answering 3 questions, but they need to be relevant questions. The example that was given to us as a bad question was: "does sq. ft. (living space) affect price?" This is not a bad question because it cant be answered clearly (or super easily), but because it doesn't really teach us anything either. We already know sq. ft. affects housing prices. This was particularly challenging for me, considering I only started "coding" less than a month ago. At this point I stopped and really started 
"Pouring" though the data again, as well as searching the Web for similar research that had been done, and trying to choose the best questions to ask.  

### Q1: Is there a correlation between sales and day of the week?

Data Handling


```python
df['weekday'] = df['date'].dt.dayofweek

df.weekday.hist()
plt.show()
```


![png](student_files/student_40_0.png)


Visualization


```python
# rewrite the column using strings I can easily read
dayOfWeek={0:'Monday', 1:'Tuesday', 2:'Wednesday', 3:'Thursday', 4:'Friday', 5:'Saturday', 6:'Sunday'}
df['day'] = df['date'].dt.dayofweek.map(dayOfWeek)

day_counts = df['day'].value_counts()

plt.figure(figsize=(14,8))
sns.barplot(day_counts.index, day_counts.values, alpha=0.6,)
plt.title('Weekday Homes Are Bought?')
plt.ylabel('Sum of Sales', fontsize=10)
plt.xlabel('Day of The Week', fontsize=10)
plt.show()
```


![png](student_files/student_42_0.png)


### Conclusion 

Tuesday seems to be the day of the week when most homes are bought. It appears to build up slightly from Monday, to Tuesday, then taper slowly down until the weekend where it drops off drastically (no surprise, banks are closed through the weekend). 

**Recommendation**

If you work in real estate, you now know Tuesday is a day you cant afford to miss. If you are a potential home buyer, maybe you will avoid trying to schedule a bunch of "walkthroughs" on Tuesday, as you are less likely to get a realtors undivided attention. If you invest in property on a regular basis, it may benefit you to schedule your document signing (closings) on Fridays, as these are typically the lightest days for the banks. If I had more business acumen for this particular industry, I would be able to come up with better uses for this information, but I am confident that it will be relevant to a number of individuals.

### Q2: What affect does number of bedrooms have on homes?
I can think of a number of reasons this would be a good question. Having spent some time as a general contractor, I would be curious to know what type of house (bedroom wise) is selling most right now. there is no point developing a whole new subdivision of homes that you wont be able to sell off quickly.  


```python
df['bedrooms'].value_counts().plot(kind='bar',)
plt.title('The Number of Bedrooms')
plt.xlabel('Bedrooms')
plt.ylabel('Count')
sns.despine 
# Seaborn.despine() negates the effect of moving the y-axis to the right hand side
# of the figure, not seeing difference, but left it in anyways.
plt.show()
```


![png](student_files/student_46_0.png)


### Conclusion
It appears that homes with 2 - 5 bedrooms take up the majority of homes in this area, and that 3 - 4 bedrooms are the most common, and likely popular/sought after homes right now.

**Recommendation**

So if I was still a contractor, or even better, a developer, I would want to squeeze as many 3-4 bedroom houses on my new lot as possible. *Putting too many houses in a single development without spacing them out will actually lower value, but we are not getting into that right now. * I think it is also safe to say that if you are in the market for a house with more than 6 or even 5 bedrooms, your options will be somewhat limited if you confine your search to this specific area. I think there may also be a potential for an untapped, or underrepresented portion of the population as well. I don't really know the specifics of this area, in the sense that I have not actually visited, walked down the streets, etc. despite that, I think there is a almost a lack of small, 1 - 2 bedroom homes (definitely 1 bedroom). not even taking into account that there are a lot more young, single professionals that want to own these days (which there are), there is a big push towards minimalism (tiny homes). I admit that I thought this was just a trend (it still might be), but it doesn’t seem to be going away. With that in mind, if I was looking for investors, I think I would try to pitch developing some smaller "Hip" housing communities, which build high grade, smaller, more eclectic (architectural) homes. 

One of the first things that stood out to me about this data set was the fact that they had the latitude and longitude for each home. So I obviously wanted to see if I could plot these values, and hopefully gain some insight in the process.

### Q3: Can we draw any conclusions from the spread of homes geographically in this area ?


```python
plt.figure(figsize=(8,8))
sns.jointplot(x=df.lat.values, y=df.long.values, height=8)
plt.ylabel('Longitude', fontsize=10)
plt.xlabel('Latitude', fontsize=10)
sns.despine
plt.show()
```


    <Figure size 576x576 with 0 Axes>



![png](student_files/student_50_1.png)


### Conclusion
Yes, I believe we can. I can see fairly quickly that certain areas have particularly dense spread of homes. Areas like -122.2 - -122.4 (longitude) appear to be particularly popular to build homes, especially if you also take into account where those ranges intersect with more densely populated latitudes (like 47.7).
 **Recommendation**
 
The first thing that comes to mind is if someone where purchasing property. This could be to develop more homes, or simply purchase the land and sit for a few years as the area continues to grow in popularity, and value. If you were clever, you may even focus on the areas between where homes are densely clustered, evaluate the area in person (due diligence), and decide if these areas will likely also build up as the established "popular" location inevitably fill in, get overcrowded, and people start looking to spread out to the surrounding areas.  

[Back to Introduction](#Introduction)


```python
#I had to drop these(bool values) to get my scatter matrix(below) to work again. 
df = df.drop(['waterfront'], axis=1)
df = df.drop(['was_renovated'], axis=1)
df = df.drop(['has_basement'], axis=1)
```

# Modeling
Before we start attempting to model and make predictions with our data, it helps to see if we can spot any correlation in our variables. 


```python
pd.plotting.scatter_matrix(df, alpha=0.6, figsize=(15, 15))
plt.tight_layout()
plt.show()
```


![png](student_files/student_55_0.png)



```python
corr = df.corr()
plt.figure(figsize=(15, 15))
sns.heatmap(corr, annot=True, fmt='0.2g', cmap=sns.color_palette('Blues'))
plt.tight_layout()
plt.show()
```


![png](student_files/student_56_0.png)


From what I am seeing, bathrooms, grade, sq. ft. above and sqft_living15 all have strong correlations to price.!


```python
#Building a function to normalize 
def z_score(arr):
    return (arr - arr.mean())/arr.std()
```


```python
# these are the predictors I want to focus on
df.bathrooms = z_score(df.bathrooms)
df.grade = z_score(df.grade)
df.sqft_above = z_score(df.sqft_above)
df.sqft_living15 = z_score(df.sqft_living15)
```


```python
plt.figure(figsize=(10,10))
plt.hist(df.bathrooms, color='b', alpha=0.5, label='bathrooms', bins=20)
plt.hist(df.grade, color='r', alpha=0.5, label='grade', bins=20)
plt.hist(df.sqft_above, color='g', alpha=0.5, label='sqft_above', bins=20)
plt.hist(df.sqft_living15, color='y', alpha=0.5, label='sqft_living15', bins=20)
plt.legend()
plt.show()
```


![png](student_files/student_60_0.png)


Our distributions do skew to the right fairly heavily, but that was expected. Nicer, more expensive homes have more bedrooms, bathrooms, sq. ft., etc., but no matter how sad the home, it never has negative bathrooms/bedrooms...


```python
df['grade'] = df['grade'].astype(float)
```


```python
# create x and y
x = df['sqft_above'] 
y = df['price']
```


```python
linreg = sm.OLS(y,x).fit()
linreg.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared:         </th>  <td>   0.116</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.116</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   2830.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 17 May 2019</td> <th>  Prob (F-statistic):</th>   <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>06:28:00</td>     <th>  Log-Likelihood:    </th> <td>-3.1850e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 21597</td>      <th>  AIC:               </th>  <td>6.370e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 21596</td>      <th>  BIC:               </th>  <td>6.370e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
       <td></td>         <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>sqft_above</th> <td> 2.224e+05</td> <td> 4180.558</td> <td>   53.197</td> <td> 0.000</td> <td> 2.14e+05</td> <td> 2.31e+05</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>16492.245</td> <th>  Durbin-Watson:     </th>  <td>   0.450</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>728366.432</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 3.265</td>   <th>  Prob(JB):          </th>  <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td>30.691</td>   <th>  Cond. No.          </th>  <td>    1.00</td> 
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



[Back to Introduction](#Introduction)

# Investigate models

p-value is 0, sqft_above has a Coefficient 2.224. If I understand this correctly, it means we can say with a good amount of certainty, that sqft_above affects roughly 11%(rounded R-squared) of price (target).


```python
#Different iterations using the other chosen predictor variables.
x = df['sqft_living15'] 
y = df['price']
linreg = sm.OLS(y,x).fit()
linreg.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared:         </th>  <td>   0.108</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.108</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   2622.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 17 May 2019</td> <th>  Prob (F-statistic):</th>   <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>06:28:00</td>     <th>  Log-Likelihood:    </th> <td>-3.1859e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 21597</td>      <th>  AIC:               </th>  <td>6.372e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 21596</td>      <th>  BIC:               </th>  <td>6.372e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
        <td></td>           <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>sqft_living15</th> <td>  2.15e+05</td> <td> 4198.430</td> <td>   51.209</td> <td> 0.000</td> <td> 2.07e+05</td> <td> 2.23e+05</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>20143.282</td> <th>  Durbin-Watson:     </th>  <td>   0.462</td>  
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>1910578.895</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 4.207</td>   <th>  Prob(JB):          </th>  <td>    0.00</td>  
</tr>
<tr>
  <th>Kurtosis:</th>       <td>48.303</td>   <th>  Cond. No.          </th>  <td>    1.00</td>  
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.




```python
x = df['bathrooms'] 
y = df['price']
linreg = sm.OLS(y,x).fit()
linreg.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared:         </th>  <td>   0.087</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.087</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   2069.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 17 May 2019</td> <th>  Prob (F-statistic):</th>   <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>06:28:00</td>     <th>  Log-Likelihood:    </th> <td>-3.1884e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 21597</td>      <th>  AIC:               </th>  <td>6.377e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 21596</td>      <th>  BIC:               </th>  <td>6.377e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
      <td></td>         <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>bathrooms</th> <td> 1.932e+05</td> <td> 4247.215</td> <td>   45.489</td> <td> 0.000</td> <td> 1.85e+05</td> <td> 2.02e+05</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>17251.570</td> <th>  Durbin-Watson:     </th>  <td>   0.491</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>882735.889</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 3.452</td>   <th>  Prob(JB):          </th>  <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td>33.550</td>   <th>  Cond. No.          </th>  <td>    1.00</td> 
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.




```python
x = df['grade'] 
y = df['price']
linreg = sm.OLS(y,x).fit()
linreg.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared:         </th>  <td>   0.141</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.141</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   3546.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 17 May 2019</td> <th>  Prob (F-statistic):</th>   <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>08:41:52</td>     <th>  Log-Likelihood:    </th> <td>-3.1818e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 21597</td>      <th>  AIC:               </th>  <td>6.364e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 21596</td>      <th>  BIC:               </th>  <td>6.364e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
    <td></td>       <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>grade</th> <td> 2.454e+05</td> <td> 4120.567</td> <td>   59.551</td> <td> 0.000</td> <td> 2.37e+05</td> <td> 2.53e+05</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>19879.964</td> <th>  Durbin-Watson:     </th>  <td>   0.401</td>  
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>2043898.709</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 4.081</td>   <th>  Prob(JB):          </th>  <td>    0.00</td>  
</tr>
<tr>
  <th>Kurtosis:</th>       <td>49.954</td>   <th>  Cond. No.          </th>  <td>    1.00</td>  
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



**Now let us try running all 4 predictors variables at once**


```python
x = df[['grade', 'bathrooms', 'sqft_above','sqft_living15']]
# Have the constant built in, but not running in this iteration(line below).
#x = sm.add_constant(x)
y = df['price']
linreg = sm.OLS(y,x).fit()
linreg.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared:         </th>  <td>   0.152</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.152</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   971.4</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 17 May 2019</td> <th>  Prob (F-statistic):</th>   <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>08:50:34</td>     <th>  Log-Likelihood:    </th> <td>-3.1804e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 21597</td>      <th>  AIC:               </th>  <td>6.361e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 21593</td>      <th>  BIC:               </th>  <td>6.361e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     4</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
        <td></td>           <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>grade</th>         <td> 1.495e+05</td> <td> 6984.187</td> <td>   21.402</td> <td> 0.000</td> <td> 1.36e+05</td> <td> 1.63e+05</td>
</tr>
<tr>
  <th>bathrooms</th>     <td> 2.789e+04</td> <td> 5921.210</td> <td>    4.710</td> <td> 0.000</td> <td> 1.63e+04</td> <td> 3.95e+04</td>
</tr>
<tr>
  <th>sqft_above</th>    <td> 4.868e+04</td> <td> 7311.204</td> <td>    6.658</td> <td> 0.000</td> <td> 3.43e+04</td> <td>  6.3e+04</td>
</tr>
<tr>
  <th>sqft_living15</th> <td> 5.678e+04</td> <td> 6440.084</td> <td>    8.816</td> <td> 0.000</td> <td> 4.42e+04</td> <td> 6.94e+04</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>19141.980</td> <th>  Durbin-Watson:     </th>  <td>   0.380</td>  
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>1752289.555</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 3.866</td>   <th>  Prob(JB):          </th>  <td>    0.00</td>  
</tr>
<tr>
  <th>Kurtosis:</th>       <td>46.445</td>   <th>  Cond. No.          </th>  <td>    3.61</td>  
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



**running all predictors at once did not help R-squared nearly as much as we would hope**



```python
x = df[['grade', 'bathrooms', 'sqft_above','sqft_living15']]
# this time, we will add a constant(line below).
x = sm.add_constant(x)
y = df['price']
linreg = sm.OLS(y,x).fit()
linreg.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared:         </th>  <td>   0.482</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.482</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   5030.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 17 May 2019</td> <th>  Prob (F-statistic):</th>   <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>08:51:59</td>     <th>  Log-Likelihood:    </th> <td>-3.0028e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 21597</td>      <th>  AIC:               </th>  <td>6.006e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 21592</td>      <th>  BIC:               </th>  <td>6.006e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     4</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
        <td></td>           <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>const</th>         <td> 5.403e+05</td> <td> 1798.681</td> <td>  300.385</td> <td> 0.000</td> <td> 5.37e+05</td> <td> 5.44e+05</td>
</tr>
<tr>
  <th>grade</th>         <td> 1.495e+05</td> <td> 3069.069</td> <td>   48.705</td> <td> 0.000</td> <td> 1.43e+05</td> <td> 1.55e+05</td>
</tr>
<tr>
  <th>bathrooms</th>     <td> 2.789e+04</td> <td> 2601.964</td> <td>   10.720</td> <td> 0.000</td> <td> 2.28e+04</td> <td>  3.3e+04</td>
</tr>
<tr>
  <th>sqft_above</th>    <td> 4.868e+04</td> <td> 3212.770</td> <td>   15.151</td> <td> 0.000</td> <td> 4.24e+04</td> <td>  5.5e+04</td>
</tr>
<tr>
  <th>sqft_living15</th> <td> 5.678e+04</td> <td> 2829.973</td> <td>   20.062</td> <td> 0.000</td> <td> 5.12e+04</td> <td> 6.23e+04</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>19141.980</td> <th>  Durbin-Watson:     </th>  <td>   1.969</td>  
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>1752289.555</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 3.866</td>   <th>  Prob(JB):          </th>  <td>    0.00</td>  
</tr>
<tr>
  <th>Kurtosis:</th>       <td>46.445</td>   <th>  Cond. No.          </th>  <td>    3.61</td>  
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



This certainly helped, adding a constant variable helped raise our R-squared value from roughly 15% up to nearly 50%.


```python
x.corr()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>const</th>
      <th>grade</th>
      <th>bathrooms</th>
      <th>sqft_above</th>
      <th>sqft_living15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>const</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>grade</th>
      <td>NaN</td>
      <td>1.000000</td>
      <td>0.665838</td>
      <td>0.756073</td>
      <td>0.713867</td>
    </tr>
    <tr>
      <th>bathrooms</th>
      <td>NaN</td>
      <td>0.665838</td>
      <td>1.000000</td>
      <td>0.686668</td>
      <td>0.569884</td>
    </tr>
    <tr>
      <th>sqft_above</th>
      <td>NaN</td>
      <td>0.756073</td>
      <td>0.686668</td>
      <td>1.000000</td>
      <td>0.731767</td>
    </tr>
    <tr>
      <th>sqft_living15</th>
      <td>NaN</td>
      <td>0.713867</td>
      <td>0.569884</td>
      <td>0.731767</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



We definetly have some strong correlations going on with our pridicor variables. I will try different iterations with the model below where I drop one column at a time and see if does anything to improve our R-squared score.


```python
x = df[['grade','sqft_above','sqft_living15']]
# this time, we will add a constant(line below).
x = sm.add_constant(x)
y = df['price']
linreg = sm.OLS(y,x).fit()
linreg.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared:         </th>  <td>   0.480</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.480</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   6634.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Fri, 17 May 2019</td> <th>  Prob (F-statistic):</th>   <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>10:45:17</td>     <th>  Log-Likelihood:    </th> <td>-3.0034e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 21597</td>      <th>  AIC:               </th>  <td>6.007e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 21593</td>      <th>  BIC:               </th>  <td>6.007e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     3</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
        <td></td>           <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>const</th>         <td> 5.403e+05</td> <td> 1803.419</td> <td>  299.596</td> <td> 0.000</td> <td> 5.37e+05</td> <td> 5.44e+05</td>
</tr>
<tr>
  <th>grade</th>         <td> 1.587e+05</td> <td> 2953.595</td> <td>   53.734</td> <td> 0.000</td> <td> 1.53e+05</td> <td> 1.64e+05</td>
</tr>
<tr>
  <th>sqft_above</th>    <td> 6.023e+04</td> <td> 3034.747</td> <td>   19.845</td> <td> 0.000</td> <td> 5.43e+04</td> <td> 6.62e+04</td>
</tr>
<tr>
  <th>sqft_living15</th> <td> 5.763e+04</td> <td> 2836.296</td> <td>   20.319</td> <td> 0.000</td> <td> 5.21e+04</td> <td> 6.32e+04</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>19252.942</td> <th>  Durbin-Watson:     </th>  <td>   1.974</td>  
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>1805309.401</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 3.895</td>   <th>  Prob(JB):          </th>  <td>    0.00</td>  
</tr>
<tr>
  <th>Kurtosis:</th>       <td>47.108</td>   <th>  Cond. No.          </th>  <td>    3.20</td>  
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



This was the best result I was able to obtain from simply dropping a column (bathrooms).  

[Back to Introduction](#Introduction)

**A bit of model validation to round things off**


```python
X = df[['grade', 'bathrooms', 'sqft_above','sqft_living15']]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2)
```


```python
print(len(X_train), len(X_test), len(y_train), len(y_test))
```

    17277 4320 17277 4320



```python
linreg = LinearRegression()
linreg.fit(X_train, y_train)

y_hat_train = linreg.predict(X_train)
y_hat_test = linreg.predict(X_test)
```


```python
train_residuals = y_hat_train - y_train
test_residuals = y_hat_test - y_test
```


```python
mse_train = np.sum((y_train-y_hat_train)**2)/len(y_train)
mse_test =np.sum((y_test-y_hat_test)**2)/len(y_test)
print('Train Mean Squarred Error:', mse_train)
print('Test Mean Squarred Error:', mse_test)
```

    Train Mean Squarred Error: 68384073900.66293
    Test Mean Squarred Error: 75834259455.49101



```python
num = 20
train_err = []
test_err = []
for i in range(num):
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33)
    linreg.fit(X_train, y_train)
    y_hat_train = linreg.predict(X_train)
    y_hat_test = linreg.predict(X_test)
    train_err.append(mean_squared_error(y_train, y_hat_train))
    test_err.append(mean_squared_error(y_test, y_hat_test))
plt.scatter(list(range(num)), train_err, label='Training Error')
plt.scatter(list(range(num)), test_err, label='Testing Error')
plt.legend();
```


![png](student_files/student_87_0.png)


### Summary
From the models above, the "Prob(F-statistic):" tells us that the features (predictors/independent variables) we chose to use have coefficients that are not equal to 0 with very high confidence. We can also say with confidence that 'bathrooms', 'grade', 'sq.  Ft. above, and 'sq. ft. living' (respectively), having coefficients of 1.932, 2.454, 2.224, and 2.15; shows a positive correlation with our target. We know this is not random, because each has a p-value of 0. Therefore, each predictor when ran independently against the target predicted roughly 10% of the variance of the target(price). Our residuals do skew right or positive, meaning our regression model does favor one side of data more than the other. When we ran all 4 predictors at once our R-squared score took a bit of a dive, but by adding a constant, we were able to bring it back up. However, we were not able to bring it up as much as we would have liked. There is too much correlation between our predictor variables, and this is a difficult problem to address, or at least make any all encompassing statements about at this point and time. Higher-grade homes tend to have more bathrooms, and more sq. ft. These almost always tie together, just the same as how nicer homes are usually built around other, nicer, higher-grade homes. These are difficult correlations to break apart without losing an amount of data that would ultimately make the model useless. 

**Recommendation**
Given that these trends in the market hold, it is (relatively) safe to use ‘bathrooms', 'grade', 'sqft_above', and 'sqft_living' as predictors of future house prices.
