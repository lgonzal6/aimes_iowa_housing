## Ames Housing Data Project 2    

### Problem Statement

Generally, home sellers desire to optimize the sale price of their homes. One way to increase your home price is to make home improvements and renovations to your house so that it can be more appealing to buyers.

With the Ames housing data, I have made a regression model to isolate the relationship between house features and their quality to sale price so that home sellers can use this information to inform their renovation decisions.

With this model, home sellers can estimate the expected increase in sale price for specific house improvement projects.

### The Data

In this analysis we use the data from the the Ames Assessor's Office which was used in computing assessed values for individual residential properties sold in Ames, IA from 2006 - 2010.

For the purposes of our modeling process, the data set is split into the Train and the Test files, which contain the same number of variables, except the test dataset does not contain the target variable (saleprice).


#### Data Dictionary

The data dictionary is available [Here](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt)


### Modeling Process and Analysis

In the above analysis I explored house sale prices from Ames Iowa and created a simple linear regression model that can be used by home sellers in Ames to help guide which home renovation projects to undertake.

The modeling process was iterative, I started by creating a very simple linear regression model with numerical data that demonstrated high correlation with saleprices. I then move on to do a linear regression model that included categorical features. Adding these additional features did not add variability to the model, but still presented a low R-squared scores.

I decided to then use lasso as a feature selection tool. I added all the available features to the lasso model, and realized that lasso was not converging at the default tolerance level. However, it narrowed down 81 features to only 31 features. I ran lasso again and further narrowed down to 25 features. I used the the features returned from lasso to add categorical variables into my model, knowing that lasso had helped decrease multicollinearity and noise.

Running a simple regression model with 25 features that included most of the features returned from lasso and those of interest for potential home sellers did increase the R-square score. However, the residuals showed that the model was making a lot of errors when trying to predict home prices above \$250,000. I also saw signs of multicollinearity, for instance, there were negative coefficients for variables where the relationship is positive. One example was a negative coefficient for garage quality, which signaled that the model was confused on how to differentiate the effects of each garage-related variable. So, in order to narrow down my features, reduce noise, and reduce multicollinearity, I used lasso again.

Running lasso again allowed me to narrow down my model to 21 features. Using the left-over coefficients from lasso into a linear regression model (to maintain easy interpretation of variables), improved my model's performance. While the R-squared score remain very similar, the mean average errors score was reduced and a plot of the residuals vs. predicted values showed that the model improved its ability to predict higher house prices (at about \$350,000 the model starts doing less well). This model also features the best MAE score, and I feel satisfied using it for the purpose of advising clients. Looking at the slight curve in the residuals, we may have improved the model by adding some interaction terms, or log-transforming y, however, I prioritized maintaining a model that produced simple, and easy to explain coefficients.

### Analysis of the model's estimated coefficients

For the analysis of the coefficient, I will only focus on the coefficients for our variables of interest. The variables that are most relevant for home sellers refer to features that can be reasonably renovated for the purpose of improving sale prices.  Examples of features that cannot be easily renovated or changed include fireplace quality (which actually refers to fireplace type), and roof style. Such features would require extensive construction or may not even be changed.

Note all the coefficient explanations below are **ALL ELSE HELD CONSTANT**


|Feature|Type|Range|Coeficient|Exaplanation|
|---|---|---|---|---|
|Kitchen Quality|Ordinal|1-5|\$15580|For every unit increase in the quality of kitchen, house price increases \$15,580|
|Basement Finished Area|Ordinal|0-6|\$3,876|For every unit increase in the quality of the basement finish, house price increases \$15,580 dollars|
|Wood Deck (SF)|Continuous|--|$24.64|For every square foot of wooden deck, house price increases by \$24.64|
|Central Air|Nominal|Y/N|\$4,911|Houses with central air sale for \$4,911 more|
|Paved Driveway|Nominal|Y/N/P|\$5,191|Compared to homes with a gravel drive way, homes with paved drivewars sell for \$5,191 more|
|External Condition|Ordinal|1-5|\$18,400|For every unit increase in the exterial condition, house price increases \$18,400|
|Garage Quality|Ordinal|0-5|\$2,558|For every unit increase in the quality of garage, house price increases \$2,558|
|Electrical|Ordinal|1-5|\$316|For every unit increase in the quality of the electrical system, house price increases \$316|

**0 in the ordinal ranges mean the feature is not available


#### Conclusions and Recommendations

The final linear model is able to explain about 84% of the variability in sale prices. While the model can improve in it's R-squared score, its coefficients are very simple to explain and can provide home sellers with an estimated dollar amount on the return on their renovation investments.

The model does a really good job in predicting home values below \$350,000 and can be confidently used when advising home sellers with homes that sell at around that price or lower.

The coefficient produced by the model can help home sellers estimate the increase in home values when they add or improve certain home features. I recommend home sellers themselves use my table of coefficients so that they can compare the estimated increase in house value with the estimate that they get from their contractors and then make an educated decision on whether to add or improve the feature in questions.

For instance, if a seller is considering renovating their kitchen, they may look at the estimated coefficient for kitchen quality and see that for every unit increase in kitchen quality, the home value increases by \$15,880. So if their remodeling will bring their kitchen from avg to excellent(a jump from 3 to 5), their sale price is estimated to increase by 31,160. However the home seller should consider the cost of this renovation project and only undertake the effort if the cost is less than 31,160.
