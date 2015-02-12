# Overview

## Background
Notes on R

### Working Directory
You can set a working directory (e.g. when you try to open up a file, this is the location it looks in)

    getwd()  # Gets the current directory
    setwd()  # Sets the working directory (your default)

### Load and Save Data
You can load data using _read.csv()_ and _write.csv()_.  There's also other varations including _write.table()_

    mydata = read.csv(file="C:\\Users\\wliu\\Desktop\\myfile.csv")  # read file
    mydata = write.csv(my_dataframe, "myfile.csv", sep=",", row.names=FALSE)

### Help
R has built-in help

    help(myfunction)
    ?function

### Packages
R's capabilities can be expanded with _packages_ from _CRAN_(Comprehensive R Archive Network)

    install.packages("ggplot2", dependencies = TRUE)  # installs package 'ggplot2'
    library(ggplot2)  # only need to install once, then can just reference

Some common packages include:
    - foreign  # allows you to load data from SPSS and other formats
    - Rcmdr  # allows data entry, make sure dependencies = TRUE
    - reshape  # allows to change data from wide to long formats
    - ggplot2  # for graphing
    - pastecs  # for basic descriptive statistics of variables; e.g. stat.desc()
    - RODBC  # for conneting to databases (e.g. SQL Server)
    - WRS  # for robust tests
    - Hmisc  # for data analysis (e.g. correlations, computing sample size and power)
    - boot  # for bootstrapping samples
    - car  # for regression diagnostics
    - QuantPsyc  # to get standardized regression coefficients

### Objects and Functions
R is made up of _objects_ and _functions_.  Here's an example of creating an object (my_people) and assigning multiple names using the concatenate function _c()_ ("Will", "Laura", "Mike", "Roger")

    my_people<-c("Will", "Laura", "Mike", "Roger")  # variable can hold strings
    my_age<-c(30, 26, 33, 27)  # variable can hold numbers (no quotes)    

### DataFrames
Dataframes are objects containing variables, like worksheets in Excel.

    family<-data.frame(Name=my_people, Age=my_age)  # dataframe w/ 2 variables
    family  # display contents of the entire dataframe
    #  Name  Age
    #1 Will  30
    #2 Laura 26
    #3 Mike  33
    #4 Roger 27
    family$Name  # can reference dataframe by variable (e.g. Name)
    names()  # can list the variables in the dataframe  # Name, Age
    new_family <- family[2,c("Name")]  # rows, columns
    # Laura


### list(), cbind()
The list() and cbind() functions can also be used to combine variables (instead of dataframes)

    family<-list(my_people, my_age)  # create two lists
    # [1] "Will" "Laura" "Mike" Roger"
    # [2] 30 26 33 27
    
    family<-cbind(my_people, my_age)  # paste columns of data together
    #    my_people, my_age
    #[1,] "Will", "30"  # Notice cbind() converts from int to string
    #[2,] "Laura", "26"  # cbind() is good only for combining same data types
    #[3,] "Mike", "33"
    #[4,] "Roger", "27" 


### Useful Functions

* __rowMeans()__ - mean of a row
* __rowSums()__ - sum of a row
* __sqrt()__ - square root
* __abs()__ - absolute value
* __log10()__ - base 10 logarithm
* __log()__ - natural logarithm
* __is.na()__ - is value missing?
* __ifelse()__ - ifelse(a conditional argument, what happens if TRUE, what happens if FALSE)


### Creating your own Functions
    nameOfFunction <- function(input1, input2)
    {
        output <- input1 + input2
        cat("Output is: ", output)
    }


### Data Formatting (wide and long/molten)
The _wide format_ formats data so that each row represents data from one entity while each column represents a variable.  There is no discrimination between independent and dependent variables (each should be its own column)

    # Wide Format Example
    # person, gender, happy_base, happy_6_months, happy_1_year
    # Will,  Male, 2, 3, 4
    # Laura, Female, 5, 6, 7

The _long or molten format_ formats data so that scores on different variables (happiness over time) are placed in a single column.

    # Long or Molten Format Example
    # person, gender, variable, value
    # Will,  Male, happy_base, 2
    # Will,  Male, happy_6_months, 3
    # Will,  Male, happy_1_year, 4
    # Laura, Female, happy_base, 5
    # Laura, Female, happy_6_months, 6
    # Laura, Female, happy_1_year, 7 

To reshape the data between _wide_ and _long_ formats, we can use these functions from the _reshape_ package:

*   __melt() and cast()__
*   __stack() and unstack()__

### Filtering with by() and subset()
To separate into different groups, we can use _by()_ and _subset()_.

* __by()__ - if you want to split data into different groups, you can use _by()_ with the general form: `by(data = dataFrame, INDICES = grouping variable, FUN = a function that you want to apply to the data)`
* __subset()__ - if you want to split data into different groups, you can use _subset()_ with the general form: `subset(data = dataFrame, mycolumn==myvalue`


### Factor (aka coding variable, grouping variable)
Factors are variables that take on a limited number of different values (i.e. _categorical variables_) and is helpful in statistical modeling.  Use the functions _factor()_ (says this data is a categorical) and _levels()_ (shows the different categories).

    data = c(1,2,2,3,1,2,3,3,1,2,3,3,1)
    fdata = factor(data, levels=c(a,b,c), labels=("x","y","z")) # Levels: 1 2 3

    my_months = c("January","February","March",
              "April","May","June","July","August","September",
              "October","November","December")
    ordered_months = factor(my_months,levels=c("January","February","March",
                            "April","May","June","July","August","September",
                            "October","November","December"),ordered=TRUE)
    ordered_months  # Levels: January < February < March < April < May < June < July < August < September < October < November < December


### Plotting Graphs with ggplot2
You can do a quick plot using _qplot()_ (quick and easy) or build a plot layer by layer using _ggplot()_ (for more detailed plots).  Plots are made up of `ggplot(myData, aes(variable for x, variable for y)) + geoms() + opts() + theme()`.  For full documentation, see: http://docs.ggplot2.org/

* __aes__ - aesthetic properties, what the _geoms_ look like (e.g. size, color, shape) and where they're plotted.  Examples include:
    - __colour()__ - specify exact colors using RRGGBB system (e.g. #3366FF) for a shade of blue
    - __alpha()__ - specify transparency from 0 (for fully transparent) to 1 (fully opaque)
    - __linetype()__ - specify if line is solid (1), hashed (2), dotted(3), dot and dash (4), long hash (5), dot and long hash (6)
    - __size()__ - specify size in mm (default is 0.5); larger for larger, smaller for smaller
    - __shape()__ - an int between 0 and 25, each specifying a type of shape, e.g. hollow square (0), hollow triangle (1), + sign (2)

* __geoms__ - geometric objects, these are the visual elements of an object (e.g. bars, data points, text).  Each layer of the _geoms__ can accept additional arguments to change that layer's __aes__ (aesthetics).  Some common geoms include:
    - __geom_bar()__ - creates a layer with bars representing statistical properties
    - __geom_point()__ - creates a layer showing the data points (e.g. scatterplot)
    - __geom_line()__ - creates a layer that connects data points with a straight line
    - __geom_smooth()__ - creates a layer that contains a smoother line
    - __geom_histogram()__ - creates a layer with a histogram
    - _Note_: ggplot has some built-in _stats_ functions (e.g. _bin_ to bin data, _boxplot_ to compute the data for a boxplot, _summary_ to summarize data) that it uses behind the scenes.  You can adjust these properties manually if you'd like.  For a full list, look under 'Statistics' in the documentation.

* __position__ - this is just an argument, but a useful one.  You can make position adjustments on any object (e.g. _geom_ or _aes_) in a plot with the `position = "x"` where x can be:
    - __dodge__ - no overlap at the side
    - __stackfill__ - makes objects stack on top of each other (largest objects on bottom, smallest on top) 
    - __fill__ - standardizes everything to an equal height (1) and makes objects stack on top of each other to fill the bar
    - __identity__ - no position adjustment
    - __jitter__ - jitter points to avoid overplotting on top of each other

* __facet_grid() and facet_wrap()__ - faceting splits plots into subgroups.  
    - __facet_grid()__ - _facet grid_ splits the data by the combination of  variables (e.g. Male Introverts, Female Introverts, Male Extroverts, Female Extroverts).  The general format is `+ facet_grid(x ~ y)` where _x_ and _y_ are the variables you want to facet
    - __facet_wrap()__ - _facet wrap_ splits the data by a single variable (e.g. Satisfied, Neutral, Not Satisfied). The general format is `+ facet_wrap( ~ y, nrow = integer, ncol = integer)` where _nrow_ and _ncol_ are optional

* __stat_summary()__ - summarizes y values at every unique x.  The general form is: `stat_summary(function = x, geom = y)`
    - __fun.y = mean__ - the mean; usually used with _geom = "bar"_
    - __fun.y = median__ - the median; usually used with _geom = "bar"_
    - __fun.data = mean_cl_normal()__ - 95% confidence intervals assuming normality; usually used with _geom = "errorbar"_ or _geom = "pointrange"_
    - __fun.data = median_hilow()__ - median and upper and lower quantiles; usually used with _geom = "pointrange"_

* __ggsave()__ - lets you save the graph; E.g. `ggsave("mygraph.png", width = 2, height = 2)`

* __theme() and opts()__ - _theme()_ allows you to control the themes of how plots look, default is theme_grey().  _opts()_ allows you to controls pecific parts of the theme (e.g. just the axis.line)
    - __theme_text()__ - font, colour, size of labels
    - __theme_line()__ - colour, size, linetype for grid lines
    - __theme_segment()__ - colour, size, linetype for axis line and axis tick marks

### Parametric Statistics
A branch of statistics that assumes the data comes from a type of probability distribution and makes inferences about the parameters of the distribution.  The assumptions are:

* __normal distribution__ - different depending on the test you're using (remember sample size affects how we test for _normal distribution_).  What we want is a skew = 0 and kurtosis = 0.  We can eye-ball this with 'stat_function()' and 'Q-Q plot'; we can also quantify it with numbers using 
    - __stat_function()__ - draws a function over the current plot layer; for example, using the argument _fun = dnorm()_, which returns the probability (i.e. the density) for a given value
    - __Q-Q plot (quantile-quantile plot)__ - a _quantile_ is the proportion of cases we find below a certain value; using the `stat = "qq"` argument in a _qplot()_, we can plot the cumulative values we have in our data against the cumulative probability of a normal distribution (i.e. data is ranked and sorted).  If data is normally distributed, then the actuals scores will have the same distribution as the score so we'll get a straight diagonal line 
To get more accurate (instead of just eye-balling), we can:
    - __describe() or stat.desc()__ - We can get descriptive summaries of our data with the _describe()_ function of the 'psych' package or _stat.desc()_ function of the 'pasetcs' package.  Again, we're interested in `skew = 0` and `kurtosis = 0`, which we can then calculate the _z-score_ (so that we can compare to different samples that used different measures and so that we can see how likely our values of _skew_ and _kurtosis_ are likely to occur).
    - __z-score skewness calculation__ - the z-score for skewness can be calculated using `insert formula for z-score skewness`
    - __z-score kurtosis calculation__ - the z-score for kurtosis can be calculated using `insert formula for z-score kurtosis`
    - _Note:_ interpretting z-scores depends on the sample size.  Absolute value greater than 1.96 is significant at 'p < .05', greater than 2.58 at 'p < .01', greater than 3.29 is significant at 'p < .001'
        + _small samples (<30)_ - it's okay to look for values above 1.96
        + _large samples(>30 and <200)_ - it's okay to look for values above 2.58
        + _very large samples (_200+)_ - look at the shape's distribution visually and value of the skew and kurtosis statistics instead of calculating their significance (because they are likely to be significant even when skew and kurtosis are not too different than normal)
    - __skew.2SE and kurt.2SE__ - _stat.desc()_ gives us _skew.2SE._ and _kurt.2SE_, which stands for the _skew_ and _kurtosis_ value divided by 2 standard errors (i.e. instead of values above, we can say if the absolute value greater than 1 is significant at 'p < 0.5', greater than 1.29 at 'p < .01', greater than 1.65 is significant at 'p < .001')
    - __Shapiro-Wilk Test of Normality__ - _Shapiro-Wilk_ is a way of looking for normal distribution by checking whether the distribution as a whole deviates from a comparable normal distribution.  This is represented as _normtest.W_ (W) and _normtest.p_ (p-value) through either the _stat.desc()_ or _shapiro.test()_ functions
* __homogeneity of variance__ - variances should be the same throughout the data (i.e. data is tightly packed around the mean).  For example, say we measured the number of hours a person's ear rang after a concert across multiple concerts.
    - __homogeneity of variance__ - After each concert, the ringing lasts about 5 hours (even if this is sometimes 10-15 hours, 20-25 hours, 40-45 hours)
    - __heterogeneity of variance__ - After each concert, the ringing lasts from 5 to 30 hours (say first concert is 5-10 hours, last concert is 20-50 hours)
    - __Levene's test (F)__ - _Levene's test_ tests that the variances in different groups are equal (i.e. the difference between variances is equal to zero).
        + Test is significant at p <= .05, which we can then conclue that the variances are significantly different (meaning it is not _homogeneity of variance_)
        + Test is non-significant at p > .05, then the variances are roughly equal and it is _homogeneity of variance_
        + In _R_, we can use the _leveneTest()_ function from the _car_ package.  It looks like this general form: `leveneTest(outcome variable, grouping variable, center = median/mean)` where the outcome variable is what we want to test the variances and the grouping variable is a factor
        + _Note:_ in large samples, Leven's test can be significant even when group variances are not very different; for this reason, it should be interpreted with the _variance ratio_
    - __Hartley's Fmax (aka variance ratio)__ - the ratio of the variances between the group with the biggest variance the group with the smallest variance.  This value should be smaller than the critical values
* __interval data__ - data should be measured at least in the interval level (tested with common sense)
* __independence__ - different depending on the test you're using


### Data Cleaning (trying to make data fit into a normal distribution, and what happens when you can't)
After checking that the data was entered in correctly, you can do the following:

* __Remove the outlier__ - delete the case only if you believe this is not representative (e.g. for Age someone puts 200, pregnant male)
* __Data transformation__ - do trial and error data transformations.  If you're looking at differences between variables you must apply the same transformation to all variables.  Types of transformations include:
    - __Log transformation log(Xi)__ - taking the logarithm of a set of numbers squashes the right tail of the distribution.  Advantages is this corrects for _positive skew_ and _unequal variances_.  Disadvantage is that you can't take the log of zero or negative numbers (though you can do log(x +1) to make it positive where 1 is the smallest number to make the value positive.
    - __Square root transformation__ - Taking the square root centers your data (square root affects larger values more than smaller values).  Advantage is it corrects _positive skew_ and _unequal variances_.  Disadvantage is same as log, no square root of negative numbers
    - __Reciprocal transformation (1/Xi)__ - divide 1 by each score reduces the impact of large scores.  This reverses the score (e.g. a low score of 1 would create 1/1 = 1; a high score of 10 would create 1/10 = 0.1).  The small score becomes the bigger score.  You can avoid score reversal by reversing the scores before the transformation 1/(X highest - Xi).Advantages is this corrects _positive skew_ and _unequal variances_
    - __Reverse Score transformation__ - The above transformations can correct for _negative skew_ by reversing the scores.  To do this, subtract each score from the highest score obtained or the highest score + 1 (depending if you want lowest score to be 0 or 1).  Big scores have become small and small scores have become big.  Make sure to reverse this or interpret the variable as reversed. 
* __Change the score__ - if transformation fails, consider replacing the score (it's basically the lesser of two evils) with one of the following:
    - Change the score to be one unit above the next highest score in the data set
    - The mean plus three standard deviations (i.e. this converts back from a z-score)
    - The mean plus two standard deviations (instead of three times above)
*  __Dummy coding__ - is a way of representing groups of people using only zeros and ones.  To do this, we create several variables (one less than the number of groups we're recoding) by doing:
    -  Count the number of groups you want to recode and subtract 1
    -  Create as many new (dummy) variables as step above
    -  Choose one of your groups as a baseline; this should be your control group or the group that represents the majority of your people (because it might be interesting to compare other groups against the majority)
    -  Assign the baseline group value of 0 for all of your dummy variables
    -  For the first dummy variable, assign value of 1 to the first group that you want to compare against the baseline and 0 for all other groups
    -  Repeat above step for all dummy variables
    -  Place all your dummy variables into analysis

* __Robust test (robust statistics)__ - statistics that are not unduly affected by outliers or other small departures from model assumptions (i.e. if the distribution is not normal, then consider using a _robust test_ instead of a _data tranformation_).  These tests work using these two concepts _trimmed mean_ and _bootstrap_:
    - __trimmed mean__ - a mean based on the distribution of scores after you decide that some percentage of scores will be removed from each extreme (i.e. remove say 5%, 10%, or 20% of top and bottom scores before the mean is calculated)
        + __M-estimator__ - slightly different than _trimmed mean_ in that the _M-estimator_ determines the amount of trimming empiraccly.  Advantage is that we never over or under trim the data.  Disadvantage is sometimes _M-estimators_ don't give an answer.
    - __bootstrap__ - _bootstrap_ estimates the properties of the sampling distribution from the sample data.  It treats the sample data as a population from which smaller samples (_bootstrap samples_) are taken with replacement (i.e. puts the data back before a new sample is taken again).
    - _Note:_ _R_ has a package called _WRS_ that has these _robust tests_, including _boot()_
        + `my_object <- boot(data, function, replications)` where _data_ is the dataframe, _function_ is what you want to bootstrap, _replications_ is the number of bootstrap samples (usually 2,000)
        + _boot.ci(my_object)_ returns an estimate of bias, empirically derived standard error, and confidence intervals

### Covariance and Correlation
We can see the relationship between variables with _covariance_ and the _correlation coefficient_.

* __variance__ - remember that _variance_ of a single variable represents the average amount that the data vary from the mean
* __covariance__ - _covariance_ is a measure of how one variable changes in relation to another variable.  Positive covariance means if one variable moves in a certain direction (e.g. increases), the other variable also moves in the same direction (e.g. increases).  Negative covariance means if one variable moves in a certain direction (e.g. increases), the other variable moves in the opposite direction (e.g. decreases).
    - __deviance__ - remember that _deviance_ is the difference in value vertically (i.e. the quality of fit)
    - __cross-product deviations__ - when we multiple the _deviation_ of one variable by the corresponding _deviation_ of another variable
    - __covariance__ - the average sum of combined deviations (i.e. it's the cross-product deviation, but also divided by the number of observations, N-1)
    - _Note:_ _covariance_ is NOT a good standardized measure of the relationship between two variables because it depends on the scales of measurement (i.e. we can't say whether a covariance is particularly large relative to another data set unless both data sets used the same units).
    - __standardization__ - is the process of being able to compare one study to another using the same unit of measurement (e.g. we can't compare attitude in metres)
    - __standard deviation__ - the unit of measurement that we use for _standardization_; it is a measure of the average _deviation_ from the mean
* __correlation coefficient__ - this is the standardized covariance; we basically get the standard deviation of the first variable, multiply by the standard deviation of the second variable, and divide by the product of the multiplication.  There's two types of correlations including _bivariate_ and _partial_:
    - __bivariate correlation__ - correlation between two variables.  Can be:
        + __Pearson product-moment correlation (aka Pearson correlation coefficient)__ - for parametric data that requires interval data for both variables
        + __Spearman's rho__ - for non-parametric data (so can be used when data is non-normally distributed data) and requires only ordinal data for both variables.  This works by first ranking the data, then applying _Pearson's equestion_ to the ranks.
        + __Kendall's tau__ - for non-parametric data, similar to Spearman's, but use this when there's a small number of samples and there's a lot of tied ranks (e.g. lots of 'liked' in ordinal ranks of: dislike, neutral, like)
    - __partial correlation__ - correlation between two variables while 'controlling' the effect of one or more additional variables
    - _Note:_ values range from -1 (negatively correlated) to +1 (positively correlated)
        + +1 means the two variables are perfectly positively correlated (as one increases, the other increases by a proportional amount)
        + 0 means no linear relationship (if one variable changes, the other stays the same)
        + -1 means the two variables are perfectly negatively correlated (as one increases, the other decreases in a proportional amount)
        + __correlation matrix__ - if you want to get correlation coefficients for more than two variables (a gird of correlation coefficients)
        + __correlation test__ - if you only want a single correlation coefficient
    - _R_ to calculate correlation, we can the _Hmisc_ package, specifically with _cor()_, _cor.test()_, and _rcorr()_
    - __causaulity__ - _correlation_ does not give the direction of causality (i.e. correlation does not imply causation)
    - __third-variable problem (aka tertium quid)__ - _causality_ between two variables cannot be assumed because there may be other measured or unmeasured variables affecting the results
* __coefficient of determination (R^2)__ - a measure of the amount of variability in one variable that is shared by the other (i.e. indicates how well data fit a statistical model); this is simply the _correlation coefficient_ squared.  If we multiply this value by 100, we can say that variable A CAN (not necessarily does) account up to X% of variable B.  R
* __biserial and point-biserial correlation coefficient__ - these correlational coefficients are used when one of the two variables is _dichotomous (aka binary)_.  _point-biserial correlation_ is used when one variable is a _discrete_ dichotomy (i.e. dead or alive, can't be half-dead).  _biserial correlation_ is used when that one variable is a _continuous_ dichotomy (i.e. your grade is pass or fail, but it can have multiple levels including A+, C-, F).
* __partial correlation and semi-partial correlation (aka part correlation)__ - a _partial correlation_ is the relationship between two variables while controlling for the effects of a third variable on both variables in the original correlation.  _semi-partial correlation_ is the relationship between two variables while controlling for the effects of a third variable on only one of the variables in the original correlation.

### Regression Analysis
_Regression Analysis_ is a way of predicting an outcome variable from one predictor variable (_simple regression_) or from several predictor variables (_multiple regression_).  We fit a model to our data and use it to predict values of the _dependent variable_ from one or more _independent variables_.  *  __method of least squares_ - method to find the line of best fit, which finds the smallest _residuals_ (aka the difference, variance) between the actual data and our model
*  __regression model (aka regression line)__ is the line of best fit resulting from the _method of least squares_.  Remember that even though this is the best fitting line, the model could still be a bad fit to the data
*  __residual sum of squares (aka sum of squared residuals)__ - represents the degree of inaccuracy when the best model is fitted to the data
*  __model sum of squares__ - the improvement going from the worst fit line (which just uses the mean for every value) to the best fit line (our _regression model_).  If this value is large, then the _regression model_ has made a big improvement on how well the outcome variable can be predicted.  If this value is small, then the model isn't much of an improvement.
*  __F-statistic (aka F-ratio, F-test)__ - a test statistic determines the systematic variance divided by the unsystematic variance (i.e. a measure of how much the model has improved the prediction of the outcome compared to the level of inaccuracy of the model).  A large value (greater than at least 1) means a good model.  The _F-test_ is based on the ratio of the improvement due to the _model sum of squares_ and the difference between the model and the observed data _residual sum of squares_.  
*  __t-statistic__ - like the _F-test_, usually used in _t-tests_, helps us assess predictor variables in whether they improve our model
*  __ANOVA (analysis of variance)__ - tells us whether the overall model results in a good degree of prediction of the outcome variable (Note: it doesn't tell us about the individual contribution of variables in the model)
*  __Akaike information criterion (aka AIC)__ - measure of fit (like R^2), except that it penalizes the model for having more variables (whereas R^2 only fits the data better with more predictors).  `insert formula`.  The bigger the value means a worse fit, smaller the value means better fit.  Only compare the AIC to models of the same data (there's no reference; can't say 10 is small or big).
*  __Bayesian information criterion (aka BIC)__, a measure of fit like _AIC_, but has a larger penalty for more variables than _AIC_.  `insert formula`.  

### Selecting Predictors
If you're making a complex model, the selection of predictors and its order can have a huge impact.  Put predictors methodically, not just add a bunch and see what results happen.  Here's a few methods:
*  __Hierarchical Regression__ is where predictors are selected based on past work and the experimenter decides what order to enter the predictors into the model.  Known predictors should be entered in first in order of importance in predicting the outcome.
*  __Forced entry__ is where all predictors are carefully chosen, then forced into the model simultaneously in random order.
*  __Stepwise methods__ is where all predictors and their order are based off a mathematical criterion and has a direction of _forward, backward, both_.
    -  __Forward Direction (aka Forward Selection)__ - An initial model is defined that contains only the constant, then the computer searches for the predictor (out of the ones available) that best predicts the outcome variable.  If the model improves the ability to predict the outcome, the predictor is retained.  The next predictor selected is based on the largest semi-partial correlation (i.e. the 'new variance'; if predictor A explains 40% of the variation in the outcome variable, then there's 60% variation left unexplained.  If predictor B is measured only on the remaining 60% variation).  We stop selecting predictors when the _AIC_ goes up on the remaining variables (remember lower _AIC_ means better model)
    -  __Backward Direction (aka Backward Elimination)__ - An initial model that starts with all predictor variables, removes one at a time, stops if remaining variables makes _AIC_ go up (remember lower _AIC_ means better model).  This is the preferred method because of _suppressor effects_, which occurs when a predictor has an effect but only when another variable is held constant.  _Forward Direction_ is more likely than _Backward Direction_ to exclude predictors involved in suppressor effects (and thus make a _Type II error_)
    -  __Both Direction (aka stepwise)__ - Starts the same as _Forward Direction_, but whenever you add a predictor, a removal test of the least useful predictor is done to see if there's any redundant predictors
    - __Note:__ If you used one of the above stepwise methods to get dressed on a cold day, you might put on pants first instead of underwear.  It'll see that underwear doesn't fit now that you have pants on so it'll skip.  If you don't mind your computer doing lots of calculations, try the _all-subsets_ method.
    -  __All-subsets methods__ is where we try every combination of predictors to see which is the best fit (using a statistic called _Mallow's Cp_).  You can increase accuraccy, but the possibilities increase exponentially so calculations take much longer.
    -  __Another Note:__ There's a huge danger of over-fitting with too many variables so it's important to __cross-validate__ the model by splitting into train/test sets.  Remember, the fewer predictors the better.

### How's my model doing?
When making a model, we should check for two things 1.) how well the model fits the observed data through _outliers, residuals, influence cases_ and 2.) for _generalization_, which is how the model generalizes to other cases outside your sample).
*  __Outliers and Residuals__ - We want to look at outliers to see if a small number of cases heavily influence the entire model.  What we do is look for _residuals_, which is the error pressent in the model (smaller the value, the better the fit.  Large values mean outliers).
    - __Unstandardized residuals (normal residuals)__ are measured in the same units as the outcome variable and are difficult to interpret across different models.  We cannot define a universal cut-off point of what is an _outlier_.  Instead, we need _standardized residuals_.
    - __Standardized residuals__ are _residuals_ divided by an estimate of their _standard deviation_, which gives us the ability to compare residuals from different models using the properties of _z-scores_ to determine universal guidelines on acceptable and unacceptable values.  E.g. Normally distributed sample, 99% of z-scores should lie between -3.29 and 3.29.  Anything above or below these values are cause for concern and thus the model is a bad fit.
*  __Influential Cases__ - We should also check to see that any influential cases aren't greatly biasing the model.  We can assess the influence of a particular case using multiple methods:
    -  __Adjusted Predicted Value__ - So basically, two models are created; one without a particular case and with the case.  The models are then compared to see if the predicted value is the same regardless of whether the value is included.  If the predicted value is the same, the model is good.  If the predicted value is not the same, the model is a bad fit.  The _adjusted predicted value_ is the predicted value for the model without the case.  
        + __DFFit__ is the difference between the _adjusted predicted value_ (when the model doesn't include a case) from the original predicted value (when the model includes the case).
        + __DFBeta__ is the difference between a parameter estimated using all cases and estimated when one case is excluded
    -  __Studentized Residual__ is when the residual is divded by the standard error so it gives us a standardized value; this can be compared across different regression analyses because it is measured in standard units.  It's called the studentized residual because it follows a _Student's t-distribution_.  This is useful to assess the influence of a case on the ability of the model to predict the case, but doesn't provide info on how a case influences the model as a whole (i.e. the ability to predict all cases)
    -  __Cook's distance__ is a statistic that considers the effect of a single case on the model as a whole (i.e. the overall influence of a case on the model); any values greater than 1 may be cause for concern
    -  __hat values (aka leverage)__ is another way to check the influence of the observed value of the outcome variable over the predicted values (0 means the case has no influence up to 1 meaning the case has complete influence).  The formula is `insert formula`.  If no cases exert undue influence over the model, then all leverage values should be close to the average value.  Some recommend investigating cases greater than twice to three times the average.

### Assumptions in Regression Analysis
*  __Generalization__ asks the question of whether our model can generalize to other cases outside of this sample (i.e. apply to a wider population)?  We need to check for these assumptions in regression analysis.  Once these below assumptions are met, the coefficients and parameters of the regression equation are said to be _unbiased_.  An _unbiased model_ tells us that on average, the regression model from the sample is the same as the population model (but not 100%)
    -  __Variable types__ - All predictor variables must be quantitative or categorical (with two categories).  The outcome variable must be quantitative (measured at the interval level), continuous, and unbounded (no constraints on the variability of the outcome; e.g. if the outcome is a measure ranging from 1 to 10 yet the data collected vary between 3 and 7, then its constrained)
    -  __Non-zero variance__  - Predictors should have some variation in value (i.e. do not have variances of 0)
    -  __No perfect multicollinearity__ - Should be no perfect linear relationship between two or more predictors (and don't correlate too highly).
    -  __Predictors are uncorrelated with 'external variables'__ - external variables that haven't been included in the regression model which influence the outcome variable.  If external variables do correlate with the predictors, then the conclusions we draw from the model are unreliable (because other variables exist that can predict the outcome)
    -  __Homoscedasticity__ - At each level of the predictor variable(s), the variance of the residual terms should be constant.  The residuals at each level of the predictor(s) should have the same variance (_homoscedasticity_); when the variances are very unequal there is _heteroscedasticity_
    -  __Independent Errors__ - For any two observations the residual terms should be uncorrelated (i.e. independent, sometimes called lack of _autocorrelation_).
        +  __Durbin-Watson test__ is a test that checks for serial correlations between errors (i.e. whether adjacent residuals are correlated).  The test statistic can vary between 0 and 4:
            *  2 means that the residuals are uncorrelated
            *  <2 means a positive correlation between adjacent residuals
            *  >2 means a negative correlation between adjacent residuals
            *  <1 or >3 means definite cause for concern
            *  Even if value is close to 2, can still be cause for concern (this is just a quick test, still depends on sample size and model)
            *  _Note:_ this test depends on the order; if you reorder the data, you'll get a different value
    -  __Normally Distributed Errors__ - assumed that the residuals in the model are random, normally distributed variables with a mean of 0, which means the differences between the model and the observed data are most frequently zero or close to zero and that differences greater than zero are rare.  _Note:_ does not mean that predictors have to be normally distributed
    -  __Independence__ - All the values of the outcome variable are independent
    -  __Linearity__ - the mean values of the outcome variable for each increment of the predictor(s) lie along a straight line (i.e. this is a regression, we should be modeling a linear relationship)
*  __Cross-validation__ - A way to assess the accuracy of our model/ how well our model can predict the outcome in a different sample.  If a model is generalized, it can predict another sample well.  If the model is not generalized, it can't predict another sample well. 
    -  _Adjusted R^2_ - this adjusted value indicates the loss of predictive power (aka _shrinkage_).  The equation is _Stein's Formula_, `insert formula`.  Note this is different than R^2, which uses _Wherry's equation_.
    -  _Data splitting_ - Usually split data randomly to 80% train, 20% test
    -  _Sample size_ - Depends on the size of effect (i.e. how well our predictors predict the outcome), how much statistical power we want and the number of predictors.  As a general rough guide, check out Figure 7.10 in the book (Page 275)
    -  _Multicollinearity_ - exists when there is a strong relationship between two or more predictors in a regression model.  _Perfect collinearity_ exists when at least one predictor is a perfect linear combination of the others (e.g. two predictors have a correlation coefficient of 1).  As _collinearity_ increases, these three problems arise:
        +  _Untrustworthy bs_ - b coefficients increase as collinearity increase; big standard errors for b coefficients mean that bs are more variable across samples, thus b is less likely to represent the population, thus predictor equations will be unstable across samples
        +  _limits size of R_ - Having uncorrelated predictors gives you better _unique variance_
        +  _importance of predictors_ - multicollinearity makes it difficult to assess the individual importance of a predictor.  If the predictors are highly correlated then we can't tell which of say two variables is important
    -  _Testing collinearity_ - To test for collinearity, you can do the following:
        +  _correlation matrix_ shows relationships between variables to variables with anything above say above .8 as an indicator of really highly correlated and might be an issue, however it misses on detecting _multicollinearity_ since it only looks at one variable at a time
        +  _variance inflation factor (VIF)_ is a collinearity diagnostic that indicates whether a predictor has a strong linear relationship with another predictor(s) and is good for spotting relationships between multiple variables.  There's no hard and fast rules, but a 10 is a good value to start worrying or if the average _VIF_ is close to 1, then _multicollinearity_ might be biasing the model
        +  _tolerance statistic_ is the reciprocal of _variance inflation factor (i.e. 1/VIF)_ Any values below .1 are serious concerns
    -  _Plotting_ is a good way to check assumptions of regression to make sure the model generalizes beyond your sample.
* __Plotting__ - You can check assumptions quickly with graphs
    -  Graph the _standardized residuals_ against the _fitted (predicted) values_.  
        +  If the plot looks like a random array of dots, then it's good.
        +  If the dots seem to get more or less spread out over the graph (like a funnel shape) then is probably a violation of the assumption of _homogeneity of variance_.
        +  If the dots have a pattern to them (like a curved shape) then this is probably a violation of the assumption of _linearity_
        +  If the dots have a pattern and are more spread out at some points on the plot than others then this probably reflects violations of both _homogeneity of variance_ and _linearity_
    -  Graph the histogram of the residuals
        +  If the histogram looks like a normal distribution (and the Q-Q plot looks like a diagonal line) then its good
        +  If the histogram looks non-normal, then things might not be good

### Violating Linear Regression Assumptions
If assumptions are violated, then you cannot generalize your findings beyond your sample.  You can try correcting the samples using:
    -  If residuals show problems with _heteroscedasticity_ or _non-normality_, you could try transforming the raw data (though might not affect the residuals)
    -  If there's a violation of the _linearity_ assumption, then you could do a logistic regression instead
    -  You can also try a _robust regression (aka bootstrapping, robust statistics)_, which is an alternative to the _least squares regression_ when there's too many outliers or influential cases

### Logistic Regression
Logistic Regression is multiple regression, but with an outcome variable that is a categorical and predictor variables that are continuous or categorical.  There's two types of logistic regression:
*  __binary logistic regression__ is used to predict a binary response (e.g. tumor cancerous or benign).  
*  __multinomial (or polychotomous) logistic regression__ predicts more than two categories (e.g. favorite color).

### Assessing the Logistic Regression Model
There's a lot of similarities between linear and logistic regressions.
* __logit__ - In _linear/simple regression_, you predict the value of Y given X.  In _logistic regression_, you predict the probability of Y occuring given X.  You can't use _linear regression_ equations on a _logistic regression_ unless you do some data transformations (like _logit_, which logs the data)
*  __log-likelihood__ - In _linear/simple regression_, we used R^2 (the _Pearson correlation_) to check between observed values of the outcome and the values predicted by the regression model.  For _logistic regression_, we use the _log-likelihood_ given by `insert equation`, which is based on summing the probabilities associated with the predicted and actual outcomes (i.e. how much unexplained information there is after the model has been fitted).  A larger _log-likelihood_ means poor fitting model because there's more unexplained observations.
*  __maximum-likelihood estimation (MLE)__ is a method to estimate the parameters of a statistical model (i.e. the observed values most likely to have occurred)
*  __deviance (aka -2LL)__ is related to the _log-likelihood_ and its equation is `deviance =-2*log-likelihood` and sometimes used instead of the _log-likelihood_ because it has a _chi-square distribution_, which makes it easy to calculate the significance of the value.
*  __R-statistic__ is the partial correlation between the outcome variable and each of the predictor variables; can be between -1 (meaning as the the predictor value increases, likelihood of the outcome occurring decreases) to 1 (meaning that as the predictor variable increases, so does the likelihood of the event occurring).  The equation is `insert equation`
*  
