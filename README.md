# Property Value Prediction Model Analysis

## Summary

In short, this project analyzes the correlation between the number of permits issued within a zipcode and the housing values within that same zipcode.  The analysis was focused on Philadelphia, Pennsylvania and utilized permit data from https://www.opendataphilly.org/ along with housing price data from https://www.zillow.com/research/data/.

The goal of this project was to determine whether the number of permits issued in a given zip code within prior years is correlated to property value and changes of property values within the zip code.  The assumption going in is that building permits that better the buildings within a zip code should have some correlation with the future housing prices and change in housing prices.  Thus the theory is that if we're designing a machine learning model, the inclusion of permits would improve the model. 

## Analysis

(Note: More in-depth analysis included in the [Jupyter Notebook File](https://github.com/JohnnyKile/Property-Value-Prediction-Model-Analysis/blob/main/Property%20Value%20Prediction%20Model%20Analysis.ipynb))

After processing the data (see [Jupyter Notebook File](https://github.com/JohnnyKile/Property-Value-Prediction-Model-Analysis/blob/main/Property%20Value%20Prediction%20Model%20Analysis.ipynb) for further insight), the resultant DataFrame had the following columns:

- `zipcode`: zipcode
- `year`: year
- `no_permits`: number of permits issued within a given year and zipcode
- `np_1yb`, `np_3yb`, `np_5yb`: number of permits in the past year, 3 years, and 5 years respectively (rolling sums).
- `hv`: average home value within a given year and zipcode
- `hv_1yb`, `hv_3yb`, `hv_5yb`: average home value within a zipcode 1, 3, and 5 years prior to the year within the respective row.
- `pc_1yb`, `pc_3yb`, `pc_5yb`: percentage change in average home value in the past year, past 3 years, and past 5 years, respectively.
- `hv_1yf`: average home value within a zipcode 1 year into the future of the year within the respective row.
- `pc_1yf`: percentage change in average home value in the next year.

The permit data had 25 unique permit types which included mechanical permits, demolition permits, new construction permits, etc.  The analysis revealed that when using all permit types within the permit data, the correlation between the permit-related columns and house-related columns was weak to moderate, per the correlation matrix below:

![](https://github.com/JohnnyKile/Property-Value-Prediction-Model-Analysis/blob/main/images/readme-image1.png?raw=true)

### Future Property Value Models - Selected Permits

Analysis was done to find the permit types that had the highest correlation coefficient with the `hv_1yf` column which is the column for housing value one year in the future.  The permit types that had a correlation coefficient of 0.4 or higher with the `hv_1yf` column were selected and the following was the revised correlation matrix (see [Jupyter Notebook File](https://github.com/JohnnyKile/Property-Value-Prediction-Model-Analysis/blob/main/Property%20Value%20Prediction%20Model%20Analysis.ipynb) for further insight).:

![](https://github.com/JohnnyKile/Property-Value-Prediction-Model-Analysis/blob/main/images/readme-image2.png?raw=true)

A model was built utilizing linear regression after ruling out polynominal regression models as they were overfitting the data.  The first model did not use any permit data for training, while the second model used the permit columns: `no_permits`, `np_1yb`, `np_3yb`, and `np_5yb`.  RMSE values were calculated for both models.  The model that utilized permit data had about a 5% smaller error than the one without permit data.

### Future Property Value Models - Selected Permits

Analysis was done to find the permit types that had the highest correlation coefficient with the `pc_1yf` column which is the column for YOY percent change one year in the future.  The permit types that had a correlation coefficient of -0.3 or lower with the `pc_1yf` column were selected.  The only permit type that met that criteria was 'Demolition Permit'.  The following was the revised correlation matrix (see [Jupyter Notebook File](https://github.com/JohnnyKile/Property-Value-Prediction-Model-Analysis/blob/main/Property%20Value%20Prediction%20Model%20Analysis.ipynb) for further insight).:

![](https://github.com/JohnnyKile/Property-Value-Prediction-Model-Analysis/blob/main/images/readme-image3.png?raw=true)

A model was built utilizing linear regression after ruling out polynominal regression models as they were overfitting the data.  The first model did not use any permit data for training, while the second model used the permit columns: `no_permits`, `np_1yb`, `np_3yb`, and `np_5yb`.  RMSE values were calculated for both models.  The model that utilized permit data had about a 4% smaller error than the one without permit data.

### Future Property Value Models - All Permits

Given that only one permit type met the criteria for the models above, and the correlations in the matrices were similar, models were built utilizing all permit types to predict the YOY percent change one year in the future.  The following was the correlation matrix:

![](https://github.com/JohnnyKile/Property-Value-Prediction-Model-Analysis/blob/main/images/readme-image4.png?raw=true)

A model was built utilizing linear regression after ruling out polynominal regression models as they were overfitting the data.  The first model did not use any permit data for training, while the second model used the permit columns: `no_permits`, `np_1yb`, `np_3yb`, and `np_5yb`.  RMSE values were calculated for both models.  The model that utilized permit data had about a 7% smaller error than the one without permit data.

## Conclusion

The goal of this analysis, per the Introduction, was to determine whether the number of permits issued in a given zip code in prior years would help in building a model to predict future property values and property value YOY changes. This analysis showed that the addition of permit data into models predicting the property value one year into the future as well as predicting the YOY change in property value reduced the error of the model by around 4-7%, compared to models without the permit data.  

This analysis supported the general belief that the inclusion of permit information may help make models that predict housing values more accurate.

The limitations for this analysis include the following:
- The permit data available ranges back from 2007, which only gives 15 years of analysis.  
- The housing market crash of 2008 occured during the short timeframe available within our data.  This crash would be assumed to have affected housing price data and may overshadow any correlations between housing prices and permit issuance. 
- Not all renovation work on a building requires a permit, which would limit the data available within our permit dataset.  In addition, some work is occasionally performed without proper permitting, which would again limit data available in our dataset.

Our analysis dropped null values in order to properly build the models.  This meant that years 2007-2011 were dropped due to null values in the `np_5yb` column. This helped give us some separation between the analysis and the housing crash of 2008 however it limited our data to years from 2012-2021, only 10 years of data.  It's for this reason our forecast focused only on 1 year into the future. Finding correlation 3 or 5 years into the future would have produced more rows with null values and further limited our already limited date range.

### Next Steps

To take this analysis a step further it would be worth looking at the effect of housing prices and number of permits issued at zipcodes surrounding a given zipcode, rather than limiting to just within a zipcode. It would also be worth seeing if this correlation is similar in other cities that have permit data available. Lastly, going into depth as to what work the permits were specifically related to may be helpful in finding further correlations. Though there were 25 different permit types, they were large umbrellas, digging further into the data within the permit datasets may help shed light on other correlations. This wasn't done initially in order to keep this analysis more top-level given the limitations stated in the introduction, but may be worth doing in the future.

Finally, it will be helpful to revisit this analysis once more years have passed and more permit data is available to see if the relationships noted above still hold up or if they change. Having more years of data would allow for forecasting further into the future which would be more helpful.  Projections years into the future can be affected by short-term market-related issues, that can typically be smoothed out when the forecast is extended several additional years. This analysis should be revisited in several years for that reason.  That would also allow us to view if the types of permits correlated with property values and YOY changes (outlined above) remain the same or change. 
