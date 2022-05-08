# King County Real Estate Analysis

![from giphy](https://media.giphy.com/media/l0IylQoMkcbZUbtKw/giphy.gif)

Authors:  [Samira Chatrathi](https://github.com/sgchatrathi), [Andrew Choi](https://github.com/cjunhyuk), and [Richard Hinds](https://github.com/RH3421)

## Business Problem
The real estate market is undoubtedly one of high volatility. Whether it is rooted in a housing bubble or localized to a very concentrated geographic region, home value is determined by a wide-array of factors, many of which lie outside of a homeowner's control. As data scientists we strive to identify factors and develop recommendations that are highly actionable. In the current case we decided to leverage the home sales dataset of King County, Washington to identify factors that homeowners could actively modify to increase the value of their home. In this way our stakeholders would have a clear understanding of which home improvements to prioritize to maximally increase their home value.

## Data and Preparation

We used home sales data for King County, Washington which has a population of 2.25 million people. We reviewed home sales data from 2014 to 2015 which contained over 21k datapoints and cleaned our data, removing erroneous values. One limitation of using data from a single year period is that trends and effects secondary to long term influences such as inflation were difficult to account for. To prepare the data for ananlysis we decided to drop variables that pertinent for our stakeholder. We dropped were latitude, longitude, waterfront, view, square footage of interior housing living space for the nearest 15 neighbors, and the square footage of the land lots of the nearest 15 neighbors. Again, these varaibles were dropped because our stakeholder is a homeowner looking to increase the value of their home by rennovation. After we cleaned and processed our data we labeled the final dataset as 'master_dataset.csv' to use in performing the analysis. 

## Modeling

### Methods

We used Scikit Learn as well as SciPy to build our models and perform the analyses. 

### Model 0: Baseline 

Our first model was our baseline model. We used a baseline model to a reference for the performance of our future model iterations. The baseline model represented the average sale price of homes within King County within the time frame of 2014-2015.

### Model 1

Our first model contained a list of all of the continuous variables we kept from the original data set. This included Bedrooms, Bathrooms, Sqft-Lot, Floors, SqFt-Above, and Yr-built. We believed including these variables would be helpful because it would give us a holistic understanding of variables that take on a wide-array of values. Our intention with Model 1 was to identify which variables would need to be dropped. The R-squared value for Model 1 was 0.481. This tells us that our x-variables (Bedrooms, Bathrooms, Sqft-Lot, Floors, SqFt-Above, and Yr-built) accounted for 48% of variability of the sale price of a home. We also calculated the root mean squared error (RMSE) to be $258662. Secondary to the R-squared value, RMSE served as the metric of error for our analyses. We chose RMSE over Absoulte Mean Squared Error (AMSE) because AMSE performs poorly with high values. RMSE is better in terms of  performance when dealing with large error values and, in this case, housing prices.

### Model 2

For our second iteration we used only one variable Square-Foot Living. This was to see how this would effect our overall R-squared value. From the heatmap correlation table we saw that Square-Foot Living was the variable that had the highest correlation with price (0.7). After running the model, the R-squared value for Model 2 was 0.498, an improvement on Model 1. This tells us that our x-variable Square-Foot Living accounts for 50% of variability in home sale price. Our RMSE was $254336, a %1.7 increase from our prior model.

### Model 3

For our third model we wanted to take advantage of the recursive feature elimination (RFE) that we conducted after model 1 in order to prioritize which variables would be the most impactful to keep in our model. RFE is a feature selection method that fits a model while also removing the weakest features until the specified number of features to keep is achieved. We decided to keep six features in our model. Renovations are expensive and considering the most impactful variables will undoubtedly help our homeowner stakeholder manage their renovation priorities. We also included the categorical variable of condition in this model, allowing us to see the quality of materials used in a home can contribute to the overall sale price. The R-squared value of Model 3 is 0.531 which is an improvement on Model 2. This tells us that our x-variables account for 53% of variability in house sale price. However, multicollinearity between bathrooms and sqft_living might have played a role in increasing this metric. 

### Model 4

From the heat map analysis, we wanted to see which variables had high multicollinearity. Multicollinearity occurs when more than one independent variable is highly correlated with another one in the regression model. An independent variable can be predicted from another independent variable in a regression model, which can convey some inaccuracies in the overall dataset. From our heat map we saw high multicollinearity between bathrooms and sqft_living, so we decided to feature engineer a ratio of floor to bathrooms to compensate for that factor. The R-squared value of Model 4 is 0.535 which is an improvement on Model 3. This tells us that our x-variables account for 54% of variability in house sale price. 

### Final Model

For Model 5 decided to add yr_built to the independent variables used in Model 4 as we anticipated that we could improve our model by taking the age of the home into account. yr_built was also ranked as the third most important variable during our recursive feature analysis. Ultimately, our final model explored the continuous independent variables of bedrooms, floor_to_bath, sqft_living, and yr_built as well as the categorical independent variable of condition. Our final model was built to optimize R-squared with as few features as possible. 



## Regression Results

Ultimately the R-squared value of Model 5 (our final model) is 0.564 which is best for all the models run. This tells us that our x-variables account for 56% of variability in house sale price with a root mean squared error of $240,146.63. In terms of feature coefficients with all other features held constant, home values drop by $62,863.31 for every 1 bedroom, increase $60,136.22 for every 1 unit increase in floor-to-bathroom ratio, increase $347.16 for every additional square foot, increase $2,245.52 for every year the house ages, decrease $7,7720.71 if categorized as poor condition, decrease $69,601.81 if categorized as fair condition, increase $2,779.43 if categorized as good condition, and increase $4,6333.11 if categorized as very good condition.



### Post Model Assumptions


Heteroscedasticity is defined by an unequal representation of scatter. This factor is important to consider in the context of residuals because it shows a trend in the spread of the residuals. Heteroscedasticity becomes a probles within ordinary least squares regression because it assumes that all residuals are drawn from a population with constant variance. We do see a slight trend in this case. 

<img width="465" alt="Screen Shot 2022-02-18 at 8 25 43 AM" src="https://user-images.githubusercontent.com/97462844/154713961-818a13b0-1ed4-448c-a537-8f5908c8c888.png">


We can say from the image below that there is normality amongst our residuals as it is an assumption of running a linear model. Since the residuals of model are normal we can say that our assumptions are valid, so model predictions should be considered valid as well. 

<img width="425" alt="Screen Shot 2022-02-18 at 8 25 52 AM" src="https://user-images.githubusercontent.com/97462844/154713975-8ccdb0a5-f41f-4f58-80a4-4c3bf8515a22.png">


### Conclusions and Key Takeaways

Based on our analyses we recommend homeowners focus on three key actions to increase the value of their home via renovations.

1. Upgrade appliances as well as the fit and finish to improve the condition of the home.
2. Do not add bedrooms in the renovation.
3. Increase the floor-to-bathrooms ratio to further drive up the value of the home.



## For More Information
View the full analysis via the [Jupyter Notebook](https://github.com/sgchatrathi/Real-Estate-Project-analysis/blob/main/Main%20Notebook.ipynb).
