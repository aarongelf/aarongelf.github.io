---
layout: page
title: real_estate_cleaning
permalink: /real_estate_cleaning/
---


```python
import pandas as pd
import numpy as np
```


```python
df=pd.read_csv("Calgary_real_estate_data.csv")
```


```python
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>#1805 930 16 Avenue Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>$409,000</td>
      <td>$409,000</td>
      <td>100%</td>
      <td>High Rise Apartment</td>
      <td>574</td>
      <td>1</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4004 Vance Place Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>$615,000</td>
      <td>$615,000</td>
      <td>100%</td>
      <td>Half Duplex</td>
      <td>947</td>
      <td>4</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>119 Royal Oak Drive Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>September</td>
      <td>$629,900</td>
      <td>$662,000</td>
      <td>105%</td>
      <td>House</td>
      <td>1,680</td>
      <td>4</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>132 Royal Manor Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>$498,888</td>
      <td>$517,500</td>
      <td>104%</td>
      <td>Not Applicable</td>
      <td>1,403</td>
      <td>2</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>110 Cougarstone Terrace Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>December</td>
      <td>$999,000</td>
      <td>$975,000</td>
      <td>98%</td>
      <td>House</td>
      <td>2,298</td>
      <td>5</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Cleaning

df["List Price"] = (
    df["List Price"]
    .replace(r'[\$,]', '', regex=True)
    .replace('Not Applicable', np.nan)  # Replace non-numeric values
    .astype(float)  # Convert to float instead of int to handle NaNs
)
df["Sold Price"] = (
    df["Sold Price"]
    .replace(r'[\$,]', '', regex=True)
    .replace('Not Applicable', np.nan)  # Replace non-numeric values
    .astype(float)  # Convert to float instead of int to handle NaNs
)
df["Sold/List Ratio"] = (
    df["Sold/List Ratio"]
    .replace("Not Applicable", np.nan)  # Replace non-numeric values
    .str.rstrip('%')  # Remove percentage signs
    .astype(float) / 100  # Convert to decimal
)

df["Address"] = df["Address"].str.strip()
```


```python

df["SqFt"] = df["SqFt"].replace(',', '', regex=True)
df["SqFt"] = pd.to_numeric(df["SqFt"], errors="coerce")
```


```python
df.shape
```




    (9711, 11)




```python
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>#1805 930 16 Avenue Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>409000.0</td>
      <td>409000.0</td>
      <td>1.00</td>
      <td>High Rise Apartment</td>
      <td>574.0</td>
      <td>1</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4004 Vance Place Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>615000.0</td>
      <td>615000.0</td>
      <td>1.00</td>
      <td>Half Duplex</td>
      <td>947.0</td>
      <td>4</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>119 Royal Oak Drive Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>September</td>
      <td>629900.0</td>
      <td>662000.0</td>
      <td>1.05</td>
      <td>House</td>
      <td>1680.0</td>
      <td>4</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>132 Royal Manor Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>498888.0</td>
      <td>517500.0</td>
      <td>1.04</td>
      <td>Not Applicable</td>
      <td>1403.0</td>
      <td>2</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>110 Cougarstone Terrace Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>December</td>
      <td>999000.0</td>
      <td>975000.0</td>
      <td>0.98</td>
      <td>House</td>
      <td>2298.0</td>
      <td>5</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.dtypes
```




    Address             object
    Transactions        object
    Year                 int64
    Month               object
    List Price         float64
    Sold Price         float64
    Sold/List Ratio    float64
    Property Type       object
    SqFt               float64
    Beds                 int64
    Baths              float64
    dtype: object




```python
print(df.isna().sum())

```

    Address             0
    Transactions        0
    Year                0
    Month               0
    List Price          3
    Sold Price          0
    Sold/List Ratio     3
    Property Type       0
    SqFt                2
    Beds                0
    Baths              29
    dtype: int64
    


```python
df.dropna(inplace=True)
```


```python
df1=df.copy()

df1["Property Type"] = df1["Property Type"].replace("Not Applicable", np.nan)
df1.dropna(inplace=True)

df1.drop(df1[df1["SqFt"] == 0.0].index, inplace=True) #drop where sqft is 0, to prevent getting infinity when checking price/sqft
```


```python
df1.shape
```




    (8039, 11)




```python
df1.describe()
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
      <th>Year</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>8039.000000</td>
      <td>8.039000e+03</td>
      <td>8.039000e+03</td>
      <td>8039.000000</td>
      <td>8039.000000</td>
      <td>8039.000000</td>
      <td>8039.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2024.132106</td>
      <td>6.516739e+05</td>
      <td>6.419671e+05</td>
      <td>0.986368</td>
      <td>1407.046648</td>
      <td>3.157358</td>
      <td>2.180619</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.338627</td>
      <td>3.911806e+05</td>
      <td>3.785973e+05</td>
      <td>0.032399</td>
      <td>893.449258</td>
      <td>1.302855</td>
      <td>0.862949</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2024.000000</td>
      <td>1.349000e+05</td>
      <td>1.230000e+05</td>
      <td>0.700000</td>
      <td>258.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2024.000000</td>
      <td>3.950000e+05</td>
      <td>3.870000e+05</td>
      <td>0.970000</td>
      <td>905.500000</td>
      <td>2.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2024.000000</td>
      <td>6.000000e+05</td>
      <td>6.030000e+05</td>
      <td>0.980000</td>
      <td>1230.000000</td>
      <td>3.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2024.000000</td>
      <td>7.699000e+05</td>
      <td>7.600000e+05</td>
      <td>1.000000</td>
      <td>1817.000000</td>
      <td>4.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2025.000000</td>
      <td>7.900000e+06</td>
      <td>7.525000e+06</td>
      <td>1.340000</td>
      <td>55679.000000</td>
      <td>13.000000</td>
      <td>8.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#weird to have a sold/list ratio of 18, upon inspection see that a 5800 sqft house asking for 500,000 sold for 9,000,000
#assume this is a mistake and an extra 0 was added, so change the 9,000,000 to 900,000 and 18.0 to 1.8
df1[df1["Sold/List Ratio"] == 18]

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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
df1.loc[df1["Address"] == "903 Riverbend Drive Southeast, Calgary", ["Sold Price", "Sold/List Ratio"]] = [900000.0, 1.80000]

```


```python
#also see a property with 55679 sqft. Upon inspection, this was an error and should be 5567.9 sqft
df1[df1["SqFt"] > 50000]
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4282</th>
      <td>41 Cougar Plateau Point Southwest, Calgary</td>
      <td>1</td>
      <td>2025</td>
      <td>January</td>
      <td>2410000.0</td>
      <td>2410000.0</td>
      <td>1.0</td>
      <td>House</td>
      <td>55679.0</td>
      <td>6</td>
      <td>7.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.loc[df1["Address"] == "41 Cougar Plateau Point Southwest, Calgary", ["SqFt"]] = [5567.9]
```


```python
df1[df1["Address"].str.contains("Cougar Plateau", case=False, na=False)]

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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3474</th>
      <td>250 Cougar Plateau Mews Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>699000.0</td>
      <td>723000.0</td>
      <td>1.03</td>
      <td>House</td>
      <td>1539.0</td>
      <td>4</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4282</th>
      <td>41 Cougar Plateau Point Southwest, Calgary</td>
      <td>1</td>
      <td>2025</td>
      <td>January</td>
      <td>2410000.0</td>
      <td>2410000.0</td>
      <td>1.00</td>
      <td>House</td>
      <td>5567.9</td>
      <td>6</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>6816</th>
      <td>250 Cougar Plateau Way Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>September</td>
      <td>739900.0</td>
      <td>727000.0</td>
      <td>0.98</td>
      <td>House</td>
      <td>1586.0</td>
      <td>3</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>7220</th>
      <td>94 Cougar Plateau Way Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>October</td>
      <td>699000.0</td>
      <td>685000.0</td>
      <td>0.98</td>
      <td>House</td>
      <td>1625.0</td>
      <td>3</td>
      <td>2.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1[df1["Address"].str.contains("5335 84", case=False, na=False)]
#Additional 0 added to both the listing and sold price
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>872</th>
      <td>5335 84 Street Northeast, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>October</td>
      <td>4945000.0</td>
      <td>3450000.0</td>
      <td>0.7</td>
      <td>House</td>
      <td>1499.0</td>
      <td>6</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.loc[df1["Address"] == "5335 84 Street Northeast, Calgary", ["List Price", "Sold Price"]] = [494500.0,345000.0]
```


```python
df1[df1["Address"].str.contains("5120 80", case=False, na=False)]
#Additional 0 added to both the listing and sold price
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7679</th>
      <td>5120 80 Avenue Northeast, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>October</td>
      <td>6200000.0</td>
      <td>6600000.0</td>
      <td>1.06</td>
      <td>House</td>
      <td>1182.0</td>
      <td>3</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.loc[df1["Address"] == "5120 80 Avenue Northeast, Calgary", ["List Price", "Sold Price"]] = [620000.0,660000.0]
```


```python
df1.head()
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>#1805 930 16 Avenue Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>409000.0</td>
      <td>409000.0</td>
      <td>1.00</td>
      <td>High Rise Apartment</td>
      <td>574.0</td>
      <td>1</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4004 Vance Place Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>615000.0</td>
      <td>615000.0</td>
      <td>1.00</td>
      <td>Half Duplex</td>
      <td>947.0</td>
      <td>4</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>119 Royal Oak Drive Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>September</td>
      <td>629900.0</td>
      <td>662000.0</td>
      <td>1.05</td>
      <td>House</td>
      <td>1680.0</td>
      <td>4</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>110 Cougarstone Terrace Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>December</td>
      <td>999000.0</td>
      <td>975000.0</td>
      <td>0.98</td>
      <td>House</td>
      <td>2298.0</td>
      <td>5</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1634 Broadview Road Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>December</td>
      <td>1199900.0</td>
      <td>1150000.0</td>
      <td>0.96</td>
      <td>House</td>
      <td>2833.0</td>
      <td>4</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.loc[df1["Address"] == "542 23 Avenue Northwest, Calgary", ["List Price", "Sold Price"]] = [240000.0,240000.0]
```


```python
df1.isna().sum()
```




    Address            0
    Transactions       0
    Year               0
    Month              0
    List Price         0
    Sold Price         0
    Sold/List Ratio    0
    Property Type      0
    SqFt               0
    Beds               0
    Baths              0
    dtype: int64




```python
df1[df1["Sold/List Ratio"]>1.5]
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
#New columns for price per square foot and sold above/below the listing price
df1["Price/SqFt"] = df1["Sold Price"]/df1["SqFt"]
df1["Above List"] = df1["Sold Price"] > df1["List Price"]
```


```python
df1[df1["Price/SqFt"]>2376]
#again, an issue where an extra 0 was added to the listing and sold price
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
      <th>Price/SqFt</th>
      <th>Above List</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
df1.head()
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
      <th>Price/SqFt</th>
      <th>Above List</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>#1805 930 16 Avenue Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>409000.0</td>
      <td>409000.0</td>
      <td>1.00</td>
      <td>High Rise Apartment</td>
      <td>574.0</td>
      <td>1</td>
      <td>1.0</td>
      <td>712.543554</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4004 Vance Place Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>November</td>
      <td>615000.0</td>
      <td>615000.0</td>
      <td>1.00</td>
      <td>Half Duplex</td>
      <td>947.0</td>
      <td>4</td>
      <td>2.0</td>
      <td>649.419219</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>119 Royal Oak Drive Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>September</td>
      <td>629900.0</td>
      <td>662000.0</td>
      <td>1.05</td>
      <td>House</td>
      <td>1680.0</td>
      <td>4</td>
      <td>3.0</td>
      <td>394.047619</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>110 Cougarstone Terrace Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>December</td>
      <td>999000.0</td>
      <td>975000.0</td>
      <td>0.98</td>
      <td>House</td>
      <td>2298.0</td>
      <td>5</td>
      <td>3.0</td>
      <td>424.281984</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1634 Broadview Road Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>December</td>
      <td>1199900.0</td>
      <td>1150000.0</td>
      <td>0.96</td>
      <td>House</td>
      <td>2833.0</td>
      <td>4</td>
      <td>4.0</td>
      <td>405.930109</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1.to_csv("cleaned_real_estate_data.csv", index = False)
```


```python
df1.describe()
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
      <th>Year</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
      <th>Price/SqFt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>8039.000000</td>
      <td>8.039000e+03</td>
      <td>8.039000e+03</td>
      <td>8039.000000</td>
      <td>8039.000000</td>
      <td>8039.000000</td>
      <td>8039.000000</td>
      <td>8039.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2024.132106</td>
      <td>6.501575e+05</td>
      <td>6.405733e+05</td>
      <td>0.986368</td>
      <td>1400.813148</td>
      <td>3.157358</td>
      <td>2.180619</td>
      <td>463.480800</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.338627</td>
      <td>3.828068e+05</td>
      <td>3.709222e+05</td>
      <td>0.032399</td>
      <td>658.731012</td>
      <td>1.302855</td>
      <td>0.862949</td>
      <td>132.753897</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2024.000000</td>
      <td>1.349000e+05</td>
      <td>1.230000e+05</td>
      <td>0.700000</td>
      <td>258.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>162.966700</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2024.000000</td>
      <td>3.950000e+05</td>
      <td>3.860000e+05</td>
      <td>0.970000</td>
      <td>905.500000</td>
      <td>2.000000</td>
      <td>2.000000</td>
      <td>372.206861</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2024.000000</td>
      <td>5.999990e+05</td>
      <td>6.030000e+05</td>
      <td>0.980000</td>
      <td>1230.000000</td>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>430.966469</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2024.000000</td>
      <td>7.699000e+05</td>
      <td>7.600000e+05</td>
      <td>1.000000</td>
      <td>1817.000000</td>
      <td>4.000000</td>
      <td>3.000000</td>
      <td>525.749449</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2025.000000</td>
      <td>7.900000e+06</td>
      <td>7.525000e+06</td>
      <td>1.340000</td>
      <td>9578.000000</td>
      <td>13.000000</td>
      <td>8.000000</td>
      <td>1344.086022</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1[df1["Price/SqFt"]>1300]
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
      <th>Price/SqFt</th>
      <th>Above List</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2807</th>
      <td>305 33 Avenue Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>October</td>
      <td>2500000.0</td>
      <td>2500000.0</td>
      <td>1.00</td>
      <td>House</td>
      <td>1860.0</td>
      <td>3</td>
      <td>3.0</td>
      <td>1344.086022</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3920</th>
      <td>75 Bowdale Crescent Northwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>September</td>
      <td>1400000.0</td>
      <td>1200000.0</td>
      <td>0.86</td>
      <td>House</td>
      <td>901.0</td>
      <td>3</td>
      <td>1.0</td>
      <td>1331.853496</td>
      <td>False</td>
    </tr>
    <tr>
      <th>6550</th>
      <td>2639 29 Street Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>September</td>
      <td>1250000.0</td>
      <td>1250000.0</td>
      <td>1.00</td>
      <td>House</td>
      <td>952.0</td>
      <td>3</td>
      <td>1.0</td>
      <td>1313.025210</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1[df1["SqFt"]>8000]
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
      <th>Address</th>
      <th>Transactions</th>
      <th>Year</th>
      <th>Month</th>
      <th>List Price</th>
      <th>Sold Price</th>
      <th>Sold/List Ratio</th>
      <th>Property Type</th>
      <th>SqFt</th>
      <th>Beds</th>
      <th>Baths</th>
      <th>Price/SqFt</th>
      <th>Above List</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>254</th>
      <td>44 Aspen Ridge Heights Southwest, Calgary</td>
      <td>1</td>
      <td>2024</td>
      <td>December</td>
      <td>7900000.0</td>
      <td>7525000.0</td>
      <td>0.95</td>
      <td>House</td>
      <td>9578.0</td>
      <td>5</td>
      <td>8.0</td>
      <td>785.654625</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
