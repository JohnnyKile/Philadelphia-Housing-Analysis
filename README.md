# Philadelphia Housing Analysis Project

## Summary

In short, this project analyzes the correlation between the number of permits issued within a zipcode and the housing values within that same zipcode.  The analysis was focused on Philadelphia, Pennsylvania and utilized permit data from https://www.opendataphilly.org/ along with housing price data from https://www.zillow.com/research/data/. 

## Analysis

(Note: More in-depth analysis included in Jupyter Notebook file)

The goal of this analysis was to determine whether the number of permits issued in a given zip code within a given year is correlated to property value and changes of property values within the zip code.  The assumption going in was that building permits that better the buildings within a zip code should have some correlation with the future housing prices and change in housing prices.

After processing the data (see Jupyter Notebook for further insight), the resultant DataFrame had the following columns:

- `zipcode`: zipcode
- `year`: year
- `no_permits`: number of permits issued within a given year and zipcode
- `np_1y`, `np_3y`, `np_5y`: number of permits in the past year, 3 years, and 5 years respectively (rolling sum)
- `hv`: average home value within a given year and zipcode
- `pc_1y`, `pc_3y`, `pc_5y`: percentage change in home value in the past year, past 3 years, and past 5 years, respectively.

The permit data had 25 unique permit types which included mechanical permits, demolition permits, new construction permits, etc.  The analysis revealed that when using all permit types within the permit data, the correlation between the permit-related columns and house-related columns was weak, per the correlation matrix below:

![](https://github.com/JohnnyKile/Philadelphia-Housing-Analysis/blob/main/images/readme-image1.png?raw=true)

Analysis was done to find the permit types that had the highest correlation coefficient with the `hv` column (see Jupyter Notebook for further insight).  The permit types that correlated with property values tend to be later in the construction process, such as administrative permits and zoning admin permits which are related to extending deadlines, and revising previous permits.  When utilizing only those selected permit types, the number of permit-related columns and house value columns had a moderate correlation, per below:

![](https://github.com/JohnnyKile/Philadelphia-Housing-Analysis/blob/main/images/readme-image2.png?raw=true)

![](https://github.com/JohnnyKile/Philadelphia-Housing-Analysis/blob/main/images/readme-image3.png?raw=true)

Analysis was done to find the permit types that had the highest correlation coefficient with the `pc_*` columns (see Jupyter Notebook for further insight).  The permit types that correlated with percent change in property values tend to be earlier in the construction process such as site and utility work, zoning permits, and demolition permits or related to new construction or alterations such as the Residential Building Permit. When utilizing only those selected permit types, the number of permit-related columns and house value columns had a moderate correlation, per below:

![](https://github.com/JohnnyKile/Philadelphia-Housing-Analysis/blob/main/images/readme-image4.png?raw=true)

![](https://github.com/JohnnyKile/Philadelphia-Housing-Analysis/blob/main/images/readme-image5.png?raw=true)

## Conclusion

The goal of this analysis, per the Introduction, was to determine whether the number of permits issued in a given zip code within a given year is correlated to house value or changes of property values within the zip code.  The assumption going in was that building permits that better the buildings within a zip code would have some correlation with the future housing prices and change in housing prices.  This anaylsis showed that the correlation between property values vs percent change in property values depended on the types of permit used in the analysis.  

The property values within a given zipcode were moderately correlated (0.45) with permits generally issued later in the construction process, while the property value changes within a zipcode were moderately correlated (0.59) with permits issued earlier in the construction process.  This supported the general belief that work within a zipcode such a demolition work, new construction, etc., is positively correlated to the change in property values within that zipcode.

## Next Steps

To take this analysis a step further it would be worth looking at the effect of housing prices and number of permits issued at zipcodes surrounding a given zipcode, rather than limiting to just within a zipcode.  It would also be worth seeing if this correlation is similar in other cities that have permit data available.  In addition, going into depth as to what work the permits were specifically related to may be helpful as finding furhter correlations.  Though there were 25 different permit types, they were large umbrellas, digging further into the data within the permit datasets may help shed light on other correlations.  This wasn't done initially in order to keep this analysis more top-level given the limitations stated in the introduction, but may be worth doing in the future.

Going forward, it would be helpful to revisit this analysis once more years haver passed and more permit data is available to see if the relationships noted above still hold up or if they change.

