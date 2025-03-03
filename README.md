
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#1

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

## Step 1 Results

**What variables are there?**

Parcel ID, Address, Style, Occupancy, Sale Date, Sale Price, Multi-sale,
Year Built, Acres, Total Living Area (SF), Bedrooms, Finished BSMT (SQ),
Lot Area (SF), AC, Fireplace, Neighborhood

**What type are the variables?**

There is a mix of categorical (ex. Neighborhood, Occupancy, Style) and
numerical (ex. Total Living Area (SF), Bedrooms, Finished BSMT (SQ), Lot
Area (SF)). Some of the variables are continuous.

**What does each variable mean? What do we expect their data range to
be?**

Parcel ID: The house ID number in the system, it is a categorical
variable and there is no range.

Address: The address the house is located at, it is a categorical
variable and there is no range.

Style: The architectural style the house was built in, it is a
categorical variable, and the range is 12 (there are 12 different styles
the house can be classified as)

Occupancy: How many people live in the house, it is a categorical
variable, and the range is 5

Sale Date: The date the house was sold, it is a categorical variable,
and the range is any date between July 3rd, 2017 and August 31st, 2022.

Sale Price: The price the building was sold for, it is a numerical
variable, and the range is 0 - 22 million

Multi-sale: States if the house has been sold multiple times, it is a
numerical variable, and the range is 0 (no), 1 (yes).

Year Built: The year the building was built, it is a numerical variable,
and the range is 1880 to 2022 (where 0 is present as a missing value).

Acres: The total acres on the property, it is a numerical variable, and
the range is 0 to 12.

Total Living Area (SF): The total living area square footage, it is a
numerical variable, and the range is 0 to 6,007.

Bedrooms: The number of bedrooms in the property, it is a numerical
variable, and the range is 0 to 10.

Finished BSMT (SQ): The finished basement square footage, it is a
numerical variable, and the range is 10 to 6,496 (with NA for
non-finished basements).

Lot Area (SF): The square footage of the lot the house was built on, it
is a numerical variable, and the range is 0 to 523,228

AC: States whether the house has air conditioning, it is a numerical
variable, and the range is 0 (no), 1 (yes).

Fireplace: States whether the house has a fireplace, it is a numerical
variable, and the range is 0 (no), 1 (yes).

Neighborhood: What neighborhood the house is located in geographically,
it is a categorical variable and there is no range.

## Step 2 Results

The variable of interest is sales price of the houses.

## Step 3 Results

``` r
library(ggplot2)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
ggplot(ames %>% filter(`Sale Price` > 0), 
       aes(x = log(get("Sale Price")))) + 
  geom_histogram(binwidth = 1)
```

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

## Step 4 Results

Analyn’s Work:

``` r
library(classdata)
library(ggplot2)
library(dplyr)


# Scatterplot of Sale Price vs Number of Bedrooms
ggplot(ames %>% filter(`Sale Price` > 0 & Bedrooms > 0), 
       aes(x = `Bedrooms`, 
           y = log(get("Sale Price")))) +
  geom_point(alpha = 0.5, color = "blue") +
  theme_minimal() +
  labs(title = "Scatterplot of Sale Price vs Bedrooms",
       x = "Bedrooms",
       y = "Sale Price (log)")
```

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

I chose to use a scatter plot to demonstrate the relationship between
the Sale Price of a building and the number of Bedrooms that are located
in the building. I removed the zero values for both the Sale Price and
the Bedroom variables. The plot shows that the higher cost homes
typically have between 2 to 5 bedrooms, which makes sense as that is the
typical size of a house. The plot shows the 10 bedroom house sold for a
high price, which makes sense, as bigger houses typically sell for more.
There are a few outliers, where 3 and 4 bedroom homes were sold for a
significantly lower price, but this can likely be attributed to the
housing crash of 2008. To verify this, we would want to check the year
sold of these houses.

Isaac’s Work:

``` r
ggplot(ames %>% filter(`Sale Price` > 0, `Acres` > 0), 
       aes(x = `Acres`, y = log(get("Sale Price")))) +
  geom_point() + 
  labs(x = "Acres", y = "Sales Price (log)")
```

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Tirmidi’s work: \## Lot Area Impact

``` r
# Finding range of Lot Area
sum(is.na(ames$`LotArea(sf)`))
```

    ## [1] 89

``` r
colnames(ames) # Printing all column names
```

    ##  [1] "Parcel ID"             "Address"               "Style"                
    ##  [4] "Occupancy"             "Sale Date"             "Sale Price"           
    ##  [7] "Multi Sale"            "YearBuilt"             "Acres"                
    ## [10] "TotalLivingArea (sf)"  "Bedrooms"              "FinishedBsmtArea (sf)"
    ## [13] "LotArea(sf)"           "AC"                    "FirePlace"            
    ## [16] "Neighborhood"

``` r
# Scatterplot of Sale Price vs Lot Area
ggplot(ames, aes(x = `LotArea(sf)`, y = `Sale Price`)) +
  geom_point(alpha = 0.5, color = "blue") +
  theme_minimal() +
  labs(title = "Scatterplot of Sale Price vs Lot Area",
       x = "Lot Area (sq ft)",
       y = "Sale Price")
```

    ## Warning: Removed 89 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](README_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
library(ggplot2)
# library(ggthemes) # For better themes

ggplot(ames, aes(x = `LotArea(sf)`, y = `Sale Price`)) +
  geom_hex(bins = 15) +  # Adjust bins for resolution
  scale_fill_gradient(low = "blue", high = "red") +
  theme_minimal() +
  labs(title = "Hexbin Plot: Sale Price vs Lot Area",
       x = "Lot Area (sq ft)",
       y = "Sale Price")
```

    ## Warning: Removed 89 rows containing non-finite outside the scale range
    ## (`stat_binhex()`).

    ## Warning: Computation failed in `stat_binhex()`.
    ## Caused by error in `compute_group()`:
    ## ! The package "hexbin" is required for `stat_bin_hex()`.

![](README_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->
