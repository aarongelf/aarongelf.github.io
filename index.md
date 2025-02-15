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

### Integration of Geospatial Data

To get a better idea of market competitiveness we incorporated coordinate data and neighbourhood boundries from our Open Calgary datasets.  We began by ensuring consistency of address formatting between our primary dataset and the other datsets by normalizing the addresses.  This normalizing function applied standardization rules by replacing abbreviations (e.g., "St" and "ST" to "Street", "Ave" and "AVE" to "Avenue"), while removing unnecessary punctuations or unit numbers.

We then merged this cleaned coordinate data with our original dataset based on the normalized addresses, resulting in the addition of longitude, latitude, and location to our original dataset.  Unfortunately, this merge introduced some missing values, particularly for properties that had no coordinates available.  To address this, we removed rows with the missing coordinate data, ensuring that only records with complete geographical info were kept for our geospatial analysis.

Additionally, we enriched our dataset with neighbourhood names by integrating the neighbourhood Open Calgary dataset.  By converting the WKT representations of community boundaries into geometries using `shapely`, we were able to create a geodataframe that represented the Calgary community districts as multipolygons.  From this we were able to overlay neighbourhood information onto our real estae data, allowing for spatial analysis of property trends across different regions of Calgary.

## How does home size impact final sale prices?

One of the more important factors influencing a home's value is its size. We wanted to understand whether larger homes typically sell for higher prices and to what extent home size correlates with the final sale price. This is crucial for both buyers and sellers: buyers may want to know if the extra square footage justifies the higher price, and sellers might use this insight to price their properties competitively. A scatter plot was created to visualize this relationship.


![png](/assets/images/relationship_between_home_size_and_sold_price.png)


From our figure we see a positive relationship between home size and sold price, highlighting a key trend, that larger homes tend sell for higher prices.  This makes sense, as larger homes tend to have more rooms, better amenities, or additional spaces that are typically more appealing to a larger range of buyers therefore driving up their market value.  However, we do see some variation in our spread of data points, suggesting that other factors play a key role in determining the sold price besides house size.

## Which property types have the highest price per square foot?



## Are homes in Calgary selling above or below the listing price?



## What time of year sees the highest sale-to-list price ratios?




## What neighborhoods have the most competitive real estate markets?



<iframe src="/assets/calgary_property_desirability.html" width="100%" height="600"></iframe>



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
