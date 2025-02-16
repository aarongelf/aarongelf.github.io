---
layout: default
---

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](./another-page.html).

[Link to real estate data](./real_estate_cleaning/).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# Calgary Real Estate Trends: Pricing, Sales, and Market Insights

Thinking of buying a home in Calgary? For many young professionals and recent graduates, homeownership represents a key financial milestone. However, navigating the real estate market in a large city like Calgary, requires understanding pricing trends, market competitiveness, and property value over time. Rising home prices, fluctuating mortgage rates, and shifting buyer demand all play a role in shaping housing affordability.

This analysis examines Calgary’s real estate market, focusing on property sales data to uncover trends in pricing, home size, and market competitiveness. By exploring factors like price per square foot, seasonal sales patterns, and neighborhood desirability, this study provides insights that can help first-time buyers and investors make informed decisions.

Key guiding questions include:

- How does home size (square footage) impact final sale prices?
- Which property types have the highest price per square foot?
- Are homes in Calgary selling above or below the listing price?
- What time of year sees the highest sale-to-list price ratios?
- What neighborhoods have the most competitive real estate markets?
- Can we train a classification model to predict whether a house is selling at a more competitive rate?
- Can we train a regression model to predict the `Sold/List Ratio` of a house?

## Our Dataset

Our main data was initially scraped from [Bōde Canada Inc](https://bode.ca/sold-data?province=AB&city=Calgary&transactionDate=2%20years), an online real estate platform, with real estate data ranging from August 2024, to March 2025. To determine specific neighborhood information and coordinate data, we merged our data with datasets from Open Calgary that provided information on [Calgary neighborhoods](https://data.calgary.ca/Base-Maps/Community-Boundaries/ab7m-fwn6) and [Calgary coordinates](https://data.calgary.ca/Base-Maps/Parcel-Address-and-lat-long/s8b3-j88p/about_data).

Our initial dataset contained the following key attributes:

- `Address`: The property location
- `Transactions`: The number of sales per listing
- `Year/Month`: Date of the sale
- `List Price`: The asking price in Candian dollars (CAD)
- `Sold Price`: The final sale price (CAD)
- `Sold/List Ratio`: The ratio of sale price to asking price, useful for determining market competitiveness
- `Property type`: Whether the property was a house, duplex, apartment, etc.
- `SqFt`: The square footage of the house
- `Beds`: The number of beds in the house
- `Baths`: The number of baths in the house

## Data Preprocessing

To ensure our dataset was clean and suitable for analysis, several preprocessing steps were taken, through the use of several python packages such as `pandas`, `numpy`, `geopandas`, and `shapely`.

### Data Cleaning

The `List Price` and `Sold Price` columns both contained dollar signs and commas, both of which were removed to allow for numeric operations.  Following this, we made sure to convert both columns to floating-point numbers so they could missing values properly.  We also found that the `Sold/List Ratio` column had percentage signs, which were removed, followed by converting the values to decimal format for easier calculations.  Next, we found that the `SqFt` column contained comma-separated values, which were cleaned and then converted to a numeric format. Finally, the `Address` column had trailing spaces which we made sure to strip to allow for consistent text-based operations.

### Handling Missing and Erroneous Values

We began by identifying and removing missing values to maintain data integrity.  Following this, entries with a `SqFt` value of zero were removed. These entries likely represented land purchases intended for development. They are not comparable to residential properties in terms of size or price, so we felt it best to exclude them.  Lastly, outliers were identified and corrected. One example, was a property with an extremely large `Sold/List Ratio` of 18, most likely due to a data entry mistake.  Upon inspection we found a 5,800 sqft house was asking for $500,000, but sold for $9,000,000, suggesting an additional 0 was added to the `Sold Price`.

### Feature Engineering for Regression and Classification

To prepare our data for regression and classification models, we required a couple of feature engineering steps.  We began by converting categorical variables to numeric representations using label encoding through `sklearn`.  This allowed us to use neighbourhood data for classification analysis.  Continuous variables were scaled using a `StandardScaler` to ensure that all features contributed equally to the model.  This is important as logistic and linear regression are both sensitive to the scale of the data.

### Feautre Selection and Model Refinement

Before training our models, we conducted a Variance Inflation Factor (VIF) test to assess multicollinearity among our features. Our VIF analysis revealed that `List Price`, and `SqFt` had high VIF values, indicating strong correlation with other features in the dataset. In order to improve model stability and avoid multicollinearity, we decided to exclude both variables from our trained models. By removing these features, we ensured that our models focused on more independent predictors, enhancing the reliability of our predictions.

### Train-Test Split

Before we trained our models, we split the dataset into training and testing susbsets to evaluate the models performance.  This ensured our models generalized well, and did not overfit the training data.  An 80-20 split was utilized, where 80% of the data was used for training, and 20% for testing.

### Integration of Geospatial Data

To get a better idea of market competitiveness we incorporated coordinate data and neighbourhood boundries from our Open Calgary datasets.  We began by ensuring consistency of address formatting between our primary dataset and the other datsets by normalizing the addresses.  This normalizing function applied standardization rules by replacing abbreviations (e.g., "St" and "ST" to "Street", "Ave" and "AVE" to "Avenue"), while removing unnecessary punctuations or unit numbers.

We then merged this cleaned coordinate data with our original dataset based on the normalized addresses, resulting in the addition of longitude, latitude, and location to our original dataset.  Unfortunately, this merge introduced some missing values, particularly for properties that had no coordinates available.  To address this, we removed rows with the missing coordinate data, ensuring that only records with complete geographical info were kept for our geospatial analysis.

Additionally, we enriched our dataset with neighbourhood names by integrating the neighbourhood Open Calgary dataset.  By converting the WKT representations of community boundaries into geometries using `shapely`, we were able to create a geodataframe that represented the Calgary community districts as multipolygons.  From this we were able to overlay neighbourhood information onto our real estae data, allowing for spatial analysis of property trends across different regions of Calgary.

## How does home size impact final sale prices?

One of the more important factors influencing a home's value is its size. We wanted to understand whether larger homes typically sell for higher prices and to what extent home size correlates with the final sale price. This is crucial for both buyers and sellers, as buyers may want to know if the extra square footage justifies the higher price, and sellers might use this insight to price their properties competitively.

<p align="center">
  <img src="/assets/images/relationship_between_home_size_and_sold_price.png">
</p>

From our figure we see a positive relationship between home size and sold price, highlighting a key trend, that larger homes tend sell for higher prices.  This makes sense, as larger homes tend to have more rooms, better amenities, or additional spaces that are typically more appealing to a larger range of buyers therefore driving up their market value.  To further support this figure, we determined the correlation between house size and sold price to be <b>0.81</b>, indicating a strong positive correlation.  This suggests that home size is a signifcant factor in determining the final sale. However, we do see some variation in our spread of data points, suggesting that other factors play a key role in determining the sold price besides house size.  

## Which property types have the highest price per square foot?

Different types of properties tend to have different price dynamics. For example, high-rise apartments often command higher prices per square foot compared to houses or duplexes due to factors such as location, amenities, and demand for urban living. By calculating the price per square foot for each property type, we aimed to identify which property types provide the best value relative to their size. This analysis helps buyers and investors make informed decisions based on the market trends for different property types.

<p align="center">
  <img src="/assets/images/avg_price_per_sqft.png">
</p>

From our bar plot we can see that the average price per square foot is highest for houses (<em> mean = 483.81, std = 143.08</em>), while lowest for fourplexes (<em> mean = 292.13</em>).  That being said, we can see there is no error bar for the fourplexes, and upon inspection we can see that there is only one property sold that was a fourplex, making it impossible to calculate the standard deviation.

## Are homes in Calgary selling above or below the listing price?

Understanding whether properties are generally selling above or below the listing price provides insight into the competitiveness of the market. A high Sold/List Ratio could indicate a seller’s market, where buyers are willing to pay more than the asking price, while a lower ratio could suggest a buyer’s market, where negotiations result in properties selling for less than the list price. By analyzing the Sold/List Ratio across transactions, we aimed to understand the dynamics of pricing and the general market trends in Calgary.

<p align="center">
  <img src="/assets/images/distribution_of_sold_list_ratio.png">
</p>

From the histrogram above, we can see that the distribution of the Sold/List Ratio lies slightly to the left of the break-even line (<em>mean Sold/List Ratio = 0.986</em>).  This distribution reveals that most properties are sold slightly below the listing price, indicating a less competitive market, where buyers are less willing to pay more than the listing price.

## What time of year sees the highest sale-to-list price ratios?

The real estate market is often influenced by seasonal trends. For instance, there may be more buyer activity in the spring and summer, and fewer properties might sell during the winter months due to colder weather and holidays. By examining how the Sold/List Ratio fluctuates over time, we aimed to identify whether the market behaves differently during specific months of the year. This insight is valuable for both buyers and sellers: for example, sellers might prefer to list their homes during peak months when competition and demand are higher, while buyers may find better deals in the off-season.

<p align="center">
  <img src="/assets/images/seasonal_trends.png">
</p>

Our line plot shows the monthly average Sold/List Ratios, highlighting seasonal trends in property sales. We observed that properties were more likely to have a higher sold/list ratio in the warmer months, making a steady decline during the colder months, and increasing again once the months began to warm up.  

Additionally, we calculated the correlation between months and sold/list ratio to be <b>-0.13</b>, indicating a weak negative correlation.  This implies that while there is a trend of properties being more likely to sell closer to or below the listing price during the colder months, the relationship is not strong enough to fully explain the behavior of the market across months.

## What neighborhoods have the most competitive real estate markets?

Real estate performance can vary greatly depending on the location within the city. By mapping the sales data geographically, we can see if certain neighborhoods consistently perform better in terms of sale prices or Sold/List Ratios. This is important for both potential buyers looking to invest in high-performing areas and sellers looking to maximize their returns. Additionally, this spatial analysis helps identify areas where the real estate market may be underperforming, offering opportunities for future investments or targeted marketing.

<iframe src="/assets/calgary_real_estate_heatmap_plasma.html" width="100%" height="600"></iframe>

TEXT

<iframe src="/assets/calgary_property_desirability.html" width="100%" height="600"></iframe>

By merging the real estate data with geographical coordinates, we created an interactive map to visualize how different neighborhoods in Calgary perform in terms of sales prices and market dynamics. This map provides a spatial understanding of the real estate landscape, showing areas with higher and lower performance.

## Can we train a classification model to predict whether a house is selling at a more competitive rate?

We aimed to train a classification model to predict whether a house is selling at a more competitive rate, defined as a Sold/List Ratio greater than 1. This classification task can help potential buyers and sellers understand the competitiveness of the market based on property characteristics. We trained a logistic regression model, using features such as price per square foot, number of bedrooms, number of bathrooms, and neighbourhood to predict the likelihood of a house being in a competitive market.

<p align="center"> 
  <img src="/assets/images/roc_curve.png">
</p>

From the logistic regression model, we achieved an <b>accuracy of 96.3%</b>, with a <b>precision of 80.4%</b> and a <b>recall of 100%</b>. The <b>ROC AUC score of 0.978</b> indicates that the model is performing very well, distinguishing between competitive and non-competitive properties. 

<p align="center"> 
  <img src="/assets/images/classification_confusion_matrix.png">
</p>

The confusion matrix further shows that the model classifies competitive houses correctly, though some non-competitive houses are occasionally misclassified. Overall, the logistic regression model is effective at predicting the competitiveness of the real estate market based on the features provided.

## Can we train a regression model to predict the Sold/List Ratio of a house?

To predict the Sold/List Ratio of a house, we trained a linear regression model. This regression model aims to estimate how much over (or under) the list price a property is likely to sell, based on factors like price per square foot, square footage, number of bedrooms, bathrooms, and the neighborhood. This analysis can help sellers better understand how their house might perform relative to the asking price and guide buyers in assessing potential deals.

<p align="center">
  <img src="/assets/images/regression_actual_vs_predicted.png"> 
</p>

Our linear regression model achieved an <strong>R<sup>2</sup> score of 0.461</strong>, indicating that approximately <b>46% of the variance</b> in the Sold/List Ratio can be explained by the model's features. The model's <b>Mean Squared Error (MSE) of 0.001 and Root Mean Squared Error (RMSE) of 0.022</b> suggest that the predictions are relatively accurate, but there is still room for improvement. The scatter plot of predicted versus actual Sold/List Ratios shows that the model is reasonably good at predicting the sold/list ratio, with some deviations indicating that other factors not included in the model may also influence the final sale-to-list price ratio.



> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
