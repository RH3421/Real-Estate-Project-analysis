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

For our third model we wanted to take advantage of the recursive feature elimination (RFE) that we conducted after model 1 in order to prioritize which variables would be the most impactful to keep in our model. RFE is a feature selection method that fits a model while also removing the weakest features until the specified number of features to keep is achieved. We decided to keep six features in our model. Renovations are expensive and considering the most impactful variables will undoubtedly help our homeowner stakeholder manage their renovation priorities. We also included the categorical variable of home condition in this model, allowing us to see how the quality of materials used in a home can contribute to the overall sale price. The R-squared value of Model 3 was 0.531 which is an improvement on Model 2. This tells us that our x-variables account for 53% of variability in home sale price. However, multicollinearity between bathrooms and sqft_living might have played a role in increasing this metric. 

### Model 4

From the heat map analysis, we wanted to see which variables had high multicollinearity. Multicollinearity occurs when more than one independent variable is highly correlated with another one in the regression model which tends to confound results. As previously mentioned, high multicollinearity between bathrooms and sqft_living existed so we decided to feature engineer a ratio of number of floor (levels in a home) to bathrooms to reduce multicollinearity. The R-squared value of Model 4 was 0.535 which is an improvement on Model 3. This tells us that our x-variables account for 54% of variability in home sale price. 

### Final Model

For Model 5 we decided to add yr_built to the independent variables used in Model 4 as we anticipated that we could improve our model by taking the age of the home into account. yr_built was also ranked as the third most important variable during our RFE analysis. Ultimately, our final model explored the continuous independent variables of bedrooms, floor_to_bath, sqft_living, and yr_built as well as the categorical independent variable of condition. Our final model was built to optimize R-squared with as few features as possible to reduce the complexity of our analysis. 

The R-squared value of Model 5 was 0.564, the best of all the models run. This tells us that our x-variables account for 56% of variability in home sale price with a root mean squared error of $240146. In terms of feature coefficients, with all other features held constant, home sale prices drop by $6286331 for every 1 bedroom, increase $60136 for every 1 unit increase in floor-to-bathroom ratio, increase $347 for every additional square foot of living space, increase $2245 for every year the house ages, decrease $77720 if categorized as poor condition, decrease $69601 if categorized as fair condition, increase $2779 if categorized as good condition, and increase $46333 if categorized as very good condition.

## Post Model Assumptions

Heteroscedasticity is identified by an unequal representation of datapoints in a scatterplot. Heteroscedasticity is important to consider in the context of model residual values because it shows a trend in the spread of the residuals. A lack of heteroscedasticity becomes an issue within ordinary least squares regression models because it assumes that all residuals are drawn from a population with constant variance. Unfortunately, in our analysis do see a slight trend toward homooscedasticityin this case. 

<img width="465" alt="Screen Shot 2022-02-18 at 8 25 43 AM" src="https://user-images.githubusercontent.com/97462844/154713961-818a13b0-1ed4-448c-a537-8f5908c8c888.png">

In the image below we can see that there is normality amongst our residuals as is assumed in the running a linear regression model. Since the residuals in model demonstrate normality we can see that the assumptions hold true and our model is valid. 

<img width="425" alt="Screen Shot 2022-02-18 at 8 25 52 AM" src="https://user-images.githubusercontent.com/97462844/154713975-8ccdb0a5-f41f-4f58-80a4-4c3bf8515a22.png">

## Conclusions and Key Takeaways

Based on our analyses we recommend homeowners focus on three key areas to increase the value of their home via renovations:

1. Upgrade appliances as well as the fit and finish to improve the condition of the home.
2. Do not add bedrooms in the renovation.
3. Increase the floor-to-bathrooms ratio to further drive up the value of the home.

## For More Information
View the full analysis via the [Jupyter Notebook](https://github.com/sgchatrathi/Real-Estate-Project-analysis/blob/main/Main%20Notebook.ipynb).
