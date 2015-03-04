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


### Linear Regression - Data Cleaning (trying to make data fit into a normal distribution, and what happens when you can't)
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

### Linear Regression - Covariance and Correlation
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

### Linear Regression Analysis
_Regression Analysis_ is a way of predicting an outcome variable from one predictor variable (_simple regression_) or from several predictor variables (_multiple regression_).  We fit a model to our data and use it to predict values of the _dependent variable_ from one or more _independent variables_.  *  __method of least squares_ - method to find the line of best fit, which finds the smallest _residuals_ (aka the difference, variance) between the actual data and our model
*  __regression model (aka regression line)__ is the line of best fit resulting from the _method of least squares_.  Remember that even though this is the best fitting line, the model could still be a bad fit to the data
*  __residual sum of squares (aka sum of squared residuals)__ - represents the degree of inaccuracy when the best model is fitted to the data
*  __model sum of squares__ - the improvement going from the worst fit line (which just uses the mean for every value) to the best fit line (our _regression model_).  If this value is large, then the _regression model_ has made a big improvement on how well the outcome variable can be predicted.  If this value is small, then the model isn't much of an improvement.
*  __F-statistic (aka F-ratio, F-test)__ - a test statistic determines the systematic variance divided by the unsystematic variance (i.e. a measure of how much the model has improved the prediction of the outcome compared to the level of inaccuracy of the model).  A large value (greater than at least 1) means a good model.  The _F-test_ is based on the ratio of the improvement due to the _model sum of squares_ and the difference between the model and the observed data _residual sum of squares_.  
*  __t-statistic__ - like the _F-test_, usually used in _t-tests_, helps us assess predictor variables in whether they improve our model
*  __ANOVA (analysis of variance)__ - tells us whether the overall model results in a good degree of prediction of the outcome variable (Note: it doesn't tell us about the individual contribution of variables in the model)
*  __Akaike information criterion (aka AIC)__ - measure of fit (like R^2), except that it penalizes the model for having more variables (whereas R^2 only fits the data better with more predictors).  `insert formula`.  The bigger the value means a worse fit, smaller the value means better fit.  Only compare the AIC to models of the same data (there's no reference; can't say 10 is small or big).
*  __Bayesian information criterion (aka BIC)__, a measure of fit like _AIC_, but has a larger penalty for more variables than _AIC_.  `insert formula`.  

### Linear Regression - Selecting Predictors
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

### Linear Regression - How's my model doing?
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

### Assumptions in Linear Regression Analysis
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

### Logistic Regression - Types
Logistic Regression is multiple regression, but with an outcome variable that is a categorical and predictor variables that are continuous or categorical.  There's two types of logistic regression:
*  __binary logistic regression__ is used to predict a binary response (e.g. tumor cancerous or benign).  
*  __multinomial (or polychotomous) logistic regression__ predicts an outcome variable that has more than two categories (e.g. favorite color).
*  There's no additional formulas between these two types of _logistic regression_; the reason is that _multinomial logistic regression_ just breaks the outcome variable down into a series of comparisons between two categories.  Say we have three outcome categories (A, B, C) then the analysis will consist of a series of two comparisons (e.g. A vs B and A vs C) or (A vs C and B vs C) or (B vs A and B vs C); basically you have to select a baseline category.

### Assessing the Logistic Regression Model
*  What are we measuring?  (A comparison between linear and logistic regressions)
*  __logit__ - In _linear/simple regression_, you predict the value of Y given X.  In _logistic regression_, you predict the probability of Y occuring given X.  You can't use _linear regression_ equations on a _logistic regression_ problem (i.e. outcome is a categorical instead of continuous) unless you do some data transformations (like _logit_, which logs the data)
*  __log-likelihood__ - In _linear/simple regression_, we used R^2 (the _Pearson correlation_) to check between observed values of the outcome and the values predicted by the regression model.  For _logistic regression_, we use the _log-likelihood_ given by `insert equation`, which is based on summing the probabilities associated with the predicted and actual outcomes (i.e. how much unexplained information there is after the model has been fitted).  A larger _log-likelihood_ means poor fitting model because there's more unexplained observations.  This is good for overall fit.  For partial fit, see _R-statistic_.
    -  __maximum-likelihood estimation (MLE)__ is a method to estimate the parameters of a statistical model (i.e. given a sample population, it estimates what most likely would have occured).  MLE gives you the coefficients most likely to have occurred.
    -  __deviance (aka -2LL)__ is related to the _log-likelihood_ and its equation is `deviance =-2*log-likelihood` and sometimes used instead of the _log-likelihood_ because it has a _chi-square distribution_, which makes it easy to calculate the significance of the value.
*  R - How the model fits the data
    -  __R-statistic__ is the partial correlation between the outcome variable and each of the predictor variables (opposed to _log-likelihood_ which is for the overall correlation instead of partial); can be between -1 (meaning as the the predictor value increases, likelihood of the outcome occurring decreases) to 1 (meaning that as the predictor variable increases, so does the likelihood of the event occurring).  The equation is `insert equation`.
    -  __Hosmer and Lemeshow's RL^2 measure__ is `insert equation` and is the proportional reduction in the absolute value of the log-likelihood measure and thus is a measure of how much the badness of fit improves as a result of the inclusion of the predictor values.  Values range from 0 (predictors are useless at predicting the outcome variable) to 1 (model predicts the outcome variable perfectly)
    -  __Cox and Snell's RCS^2__ is `insert equation` and __Nagelkerke's RN^2__ is `insert equation`, both of which are another way of getting a equivalent of R^2 in linear regression.
*  Information Criteria - Penalize a model for having more predictors
    -  __Akaike Information Criterion (AIC)__ for logistic regression is `AIC = -2LL +2k` where `-2LL` is the _deviance_ and `k` is the number of predictors
    -  __Bayes Information Criterion (BIC)__ for logistic regression is `BIC = -2LL +2k * log(n)` where `n` is the number of cases
*  How much are each predictor(s) contributing?
    -  __t-statistic__ - in linear regression, the regression coefficient _b_ and their standard errors created the _t-statistic_, which tells us how much a preditor was contributing
    -  __z-statistic (aka the Wald statistic)__ - in logistic regression, the _z-statistic_ tells us if the _b_ coefficient for that predictor is significantly different than zero.  If a coefficient _b_ is much greater than zero, then that predictor is making a significant contribution to the prediction of the outcome.  `z = b / (SEb)`.  Be warned, when the z-statistic value is large, the _standard error_ tends to be inflated, resulting in _z-statistic_ being underestimated.  An inflated _standard error_ increases the probability of rejecting a predictor as being significant when in reality it is making a significant contribution (i.e. _Type II error_)
*  So the coefficient _b_ in a _logistic regression_ is an exponent instead of multiplying, how does that work out / what does it mean?
    -  __odds__ as we normally know it is the probability of something happening / something not happening (e.g. probability becoming pregnant divided by / probability of not becoming pregnant).  This isn't the same as the _logistic regression_'s _odds ratio_ mentioned below.
    -  __odds ratio__ is the exponential of B (i.e., e^B), which is the change in odds resulting from a unit changes in the predictor.  E.g. calculate the odds of becoming pregnant when a condom is not used, calculate the odds of becoming pregnant when a condom is used, then calculate the proportionate change in odds between the two.  
        *  Formula is: `change in odds = odds after a unit change in the predictor / original odds`
        *  if the value is greater than 1, then it means as the predictor increases, the odds of the outcome occuring also increases
        *  if the value is less than 1, then it means as the predictor increases, the odds of the outcome occuring decrease
*  How do we know what order to put in / take out predictors in _logistic regression_?
    -  __forced entry method__ - the default method for conducting regression; place in one block and estimate parameters for each predictor
    -  __stepwise method__ - select a forward, backward, or both stepwise method (remember the issues with it though).
*  What are Logistic Regression assumptions?
    -  __Linearity__ - In linear regression we assume the outcome had a linear relationship with the predictors.  However, logistic regression's outcome is categorical so our linear regression assumptions don't apply.  Instead, with logistic regression we check if there are any linear relationships between any continuous predictors and the _logit_ of the outcome variable.
    -  __Independence of errors__ - Same as linear regression where the cases of data are not related
    -  __Multicollinearity__ - Same as linear regression where predictors should not be too highly correlated; can check with _tolerance statistic_ and _VIF statistics_, the eigenvalues of the scaled, uncentred cross-roducts matrix, the condition indices and the variance proportions.
*  What are situations where Logistic Regression can cause trouble?
    -  Not enough samples for specific categories (e.g. checking if people are happy, but only _n_ of 1 for people that are 80-year old, Buddhist, left-handed lesbian).  To spot, create a crosstabulations and look for really high _standard errors_.  To fix, collect more data.
    -  __complete separation__ is where the outcome variable can be perfectly predicted by one variable or a combination of variables.  This causes issues in that there's no data in the middle probabilities (where we're not very sure of the probability) which can cause a wide range of curves/probabilities.  E.g. tell between cats and burglars, there's no cats that weigh more than 15kg and no burgulars that weigh less than 30kg.  The issue is usually caused when there's too many variables fitted to too few cases.  To fix, collect more data or use a simpler model.

### Comparing two means (aka groups) using t-test
We've looked at relationships between variables, but sometimes we're interested in differences between groups of people.  This is useful in making causal inferences (e.g. two groups of people, one gets a sugar pill and other gets actual pill).  There's two different ways to expose people to experiments: _between-group_ or _independent design_ as well as exposing the same group to all experiments, just at different points in time (_repeated-measures design_).
*  __t-test__ - are used to test whether two group means are different.  There's two types including _independent-means t-test_ and _dependent-means t-test_.  _t-tests_ are basically _regressions_ so it has much of the same assumptions (_parametric tests_ based on a _normal distribution_).
    -  __independent-means t-test (aka independent-measures, independent-samples t-test)__ is used when there are two experimental conditions and different participants were assigned to each condition (e.g. Group 1 gets Treatment A, Group 2 gets Treatment B).  _independent t-tests_ also assume that scores in different treatment conditions are _independent_ (because they come from different people) and that there is _homogeneity of variance_ (but this only really matters if you have unequal group sizes.  Also if this is violated, you can use _Welch's t-test_ to adjust your data, which you should always do)
    -  __dependent-means t-test (aka matched-pairs, pair-samples t-test, paired t-test)__ - is used when there are two experimental conditions and the same participants took part in both conditions of the experiment (e.g. Group 1 gets Treatment A, Group 2 gets Treatment B, then swap with Group 1 gets Treatment B, Group 2 gets Treatment A).  The sampling distribution of the differences between scores should be a _normal distribution_ (not the scores themselves)
    - Calculations for _t-tests_ can be viewed as if the _t-test were a linear regression (GLM)_.  The _independent t-test_ can be seen as comparing the model as effect against the error.  The _dependent t-test_ can be seen as   Note: Don't really worry about the below, _R_ will do the calculations:
        *  __Generalized Linear Model (GLM), (i.e. t-test as a linear regression)__ the _t-test_ can be thought of as a form of _linear regression_.  It'll allow you to test differences between two means.  
        -  E.g. say you have two groups (one looks at picture of spider versus other that looks at a real spider and you measure their anxiety as heartrate).  The _t-test_ as a _GLM_ would setup each group (picture vs real spider) with the equation `outcome = (model) + error`.  We _dummy code_ the group variable (say Group 1 has value of 0 and Group 2 has value of 1).  We calculate the anxiety across both groups and then test whether the difference between group means is equal to 0.  The _t-statistic_ tests if the difference between group means is significantly different than zero.
        *  __Independent t-test (i.e. t-test as comparing the model or effect against the error)__  This means that when different groups participate in different conditions, pairs of scores will differ not just because of the experiment's manipulation, but also because of other sources of variance (e.g. IQ).  Therefore, we make comparisons on a 'per condition' basis'.  By looking at the 'per condition basis', we assess whether the difference between two sample means is statistically meaningful or by chance (through numerous sampling and looking at the sampling means distribution).
        *  __Dependent t-test__ Since we put the same group through multiple experiments, we need to see the score in condition A compared to condition B and add up the differences (could be large or small) for all participants.  We divide by the number of participants in the group and get the average difference (on average, how much each person's score changed in condition A to condition B).  We then compare (divide) this by the standard error (which represents if we just randomly sampled from the population and not done any experiments).  This gives us the _test statistic_ that represents the model/error. 
    __t-statistic (aka test statistic)__ - _t-tests_ produce the _t-statistic_, which tells us how extreme a statistical estimate is.  If the experiment had any kind of effect, we expect the systematic variation to be much greater than the unsystematic variation (i.e. if _t_ is much greater than 1, there's an effect; if _t_ is less than 1, there's no effect.  If _t_ exceeds the critical value for an effect, we're confident that this reflects an effect of our independent variable)
    - __effect-size and t-tests__ - even though a _t-statistic_ might not be statistically significant, it doesn't mean that our effect is unimportant.  To check if an _effect-size_ is substantive, we use the following equation: ``
    - Reporting _t-tests_ should involve stating the finding to which the test relates, report the _test statistic_, _degrees of freedom_, an estimate of the _effect-size_, and the _probability_ of that test statistic.  E.g. On average, participants experienced greater anxiety from real spiders (M = 47.00, SE = 3.18), than from pictures of spiders (M=40.00, SE = 2.68).  This difference was not significant t(21.39) = -1.68, p>.05; however, it did represent a medium-sized effect r=.34


### Comparing Several Means with ANOVA (Analysis of Variance GLM 1, aka One-way ANOVA)
If we want to compare more than two conditions, we use one-way independent _ANOVA_.  _t-tests_ checked whether two samples have the same mean while _ANOVA_ checks if three or more groups have the same means.  _ANOVA_ is an _omnibus_ test, which means it tests for an overall effect between all groups and does not say what group has a different mean.
    - __familywise (aka experimentwise error rate)__ is the error rate across statistical tests conducted on the same experimental data.  For example, we use _ANOVA_ instead of using multiple _t-tests_ on each pair of groups because the probability of _Type I_ errors would quickly stack (e.g. say .05 level of significance with 3 pairs would be .95 * .95 * .95 = .857 probability of no _Type I error_).  The more groups the more chance of an error.
    - __F-statistic (aka F-ratio)__ is the ratio of the model to its error (much like the _t-statistic_).  Say we have Groups A, B, C.  The _F-ratio_ tells us if the means of these three groups (as a whole) are different.  It can't tell what groups are different (e.g. if groups A and B are the same and C is different, if groups A, B, C are all different).  _F-ratio_ says that the experimental manipulation has some effect, but doesn't say what causes the effect.  The _F-ratio_ can be used to fit a multiple regression model and test the differences between the means (again, much like the _t-statistic_ with a linear regression)
    - When creating _dummy variables_ for _ANOVA_ groups (e.g. Group A, B, C) where we have one less dummy variable than the number of groups in the experiment.  We choose a _control group (aka baseline group)_ that acts as a baseline for other categories.  This baseline group is usually the one with no experiments (e.g. baseline is no viagra and others are low dose and high dose viagra).  When group sizes are unequal, the baseline group should have a large number of samples.
* __Assessing variation (deviance)__
At every stage of the _ANOVA_ we're assessing _variation_ (or _deviance_) from a particular model using the formula `deviation = change in (observed - model)^2`.  We calculate the fit of the most basic model, then the fit of the best model; if the model is any good then it should fit the data significantly better than the basic model.
    - __Total Sum of Squares (aka TSS, SST)__ gives us the total amount of variation.  We calculate the difference between each observed data point and the grand mean.  We then square these differences and add them together to get us the _total sum of squares_.  
        + For _TSS_, the _degrees of freedom_ is one less than the total sample size (N-1); the mean is the constant being held.  The equation is `N-1`  E.g. we have 15 participants, the degrees of freedom is 14.
    - __grand variance__ is the variance between all scores, regardless of the experimental condition, and can be used to calculate the _total sum of squares_ with the equation `<insert equation>`
    - __Model Sum of Squares (SSm)__ tells us how much of the total variation can be explained by the fact that different data points come from different groups.  We calculate the difference between the mean of each group and the grand mean, square the differences, multiply each result by the number of participants within that group, then add the values for each group together.  
        + For _SSm_, the _degrees of freedom_ is one less than the number of parameters estimated.  The equation is: `k-1`  E.g. with 3 groups of participants, we have 2 degrees of freedom.
    - __Residual Sum of Squares (SSr)__ tells us how much of the variation cannot be explained by the model (e.g. individual differences in weight, testosterone, etc in particpants).  _SSr_ can be seen by looking at the difference between the score obtained by a person and the mean of the group that the person belongs.
        + For _SSr_, the _degrees of freedom_ are the total degrees of freedom (i.e. the total sample size `N`) minus the degrees of freedom for the model (i.e. the number of groups `k`).  The equation is: `N-k`.  E.g. with 15 participants and 3 groups, we have (14 - 2 = 12) degrees of freedom. 
    - __Mean Squares (MS)__ eliminates the bias of the number of scores (because _SSm_ and _SSr_ tells us the total, not the average).  _MS_ gives us the the _sum of squares_ divided by the _degrees of freedom_.
        + _MSm_ is the average amount of variation explained by the model (the systematic variation)
        + _MSr_ is the average amount of variation explained by extraneous variables (the unsystematic variation)
        + _F-ratio_ is the measure of the ratio of the variation explained by the model and the variation explained by unsystematic factors (i.e. how good the model is against how much error there is).  The equation is: `(_MSm_)/(_MSr_)`  
    - Assumptions of _ANOVA_ include:
        + _Homogeneity of variance_ - the variances of the groups are equal.  You can check this using _Levene's test_, which is an _ANOVA test_ done on the absolute differences between the observed data and the mean or median.  If _Levene's test_ is significant (i.e. p-value is less than .05) then the variances are significantly different and we shouldn't use _ANOVA_.
        + Note that _ANOVA_ is a _robust test_, which means it doesn't matter if we break some assumptions (the _F-ratio is still accurate_).  When group sizes are equal, then _F-ratio_ is quite robust.  However, when group sizes are not equal, the accuracy of _F-ratio_ is affected by _skew_.
        + If _homogeneity of variance_ has been violated, you can try different data transformations or you can try a different version of the _F-ratio_, like _Welch's F_.
        + If there's distributional problems, there are other methods like _bootstrapping_ or _trimmed means_ and _M-estimators_ that can correct for it.
*  __Planned contrasts__ tells us which groups differ in an _ANOVA_.  This is important because _F-ratio_ tells if there's an effect, but not what group causes it.  _Planned contrasts_ are a way of comparing different groups without causing _familywise error rate_.  We can do this using two methods (_planned comparisons__ or _post hoc comparisons__):
  1.  __planned comparisons (aka planned constrasts)__ - break down the _variance_ accounted for by the model into component parts.  This is done when you have a specific hypotheses to test.  Some rules:
        -  If we have a control group, we compare this against the other groups
        -  Each comparison must compare only two 'chunks' of variation (e.g. low dose, high dose)
        -  Once a group has been singled out in a comparison, it can't be used in another comparison (we're slicing up the data like a cake, the same part of a cake can't be on multiple slices)
        -  When we're comparing two groups, we're just comparing the mean of the group to the mean of the other group
        -  E.g. We compare the average of the placebo group to the average of the low dose and high dose groups; if the standard errors are the same, the experimental group with the highest mean (high dose) will be significantly different from the mean of the placebo group.
  *  _Planned comparisons_ can be either: _Orthogonal comparisons_ or _Non-orthogonal comparisons_
  *  __Orthogonal comparisons__
    -  Now we want to answer, what groups do we compare?  Instead of creating dummy variables of 0 and 1 as we would the main _ANOVA_, we instead assign a _weight_.  Here's a few basic rules for that:
        -  Choose sensible comparisons
        -  Groups coded with positive weights will be compared against groups coded with negative weights (so assign one chunk a positive, the other a negative; it's arbitrary which one is positive or negative)
        -  The sum of weights for a comparison should be zero
        -  If a group is not involved in a comparison, it's automatically assigned a weight of 0 (which eliminates it from all calculations)
        -  For a given comparison, the weights assigned to the group in one chunk of variation should be equal to the number of groups in the opposite chunk of variation
    -  Let's go through an example of how to apply these:
        1.)  Example (Step 1): Chunk1 (Low Dose vs High Dose) vs Chunk2 (Placebo)
        2.)  Example (Step 2): Chunk1 (Positive weight) vs Chunk2 (Negative Weight)
        3.)  Example (Step 3): Chunk1 has two chunks ('Low Dose' and 'High Dose'), they each have a magnitude of 1 (for a sum of 2), weight of +1 each (for a sum of +2).  Chunk2 only has one chunk ('Placebo') so it has a magnitude of 2, weight of -2.
        4.)  The sum of the weights should add up on both sides so this means Chunk1 has weight of +1 (for Low Dose) and +1 (for High Dose) -2 (for Placebo) = `1+1-2=0`
        5.)  To compare just two groups (Low Dose and High Dose), we set the Placebo with a weight of 0 (which eliminates it from calculations).  We now have a comparison of Chunk1 (High Dose, 1 Magnitude, +1 Weight) vs Chunk2 (Low Dose, 1 Magnitude, -1 Weight).
        6.)  We now have the following dummy variables:

                  Dum_var1 (comp1), Dum_var2 (comp2), Product (comp1* comp2)
       Grp Placebo              -2,                0,                     0
       Grp Low Dose              1,               -1,                    -1
       Grp High Dose             1,                1,                     1
       Total                     0                 0                      0

        7.)  We want to do an __orthogonal__ comparison (basically make sure that _Total_ row is 0) and this means our comparisons are independent (so we can use _t-tests_).  The _p-values_ won't be correlated so we won't get _familywise errors_
        8.)  From the significance values of the _t-tests_ we can see if the experimental groups (Low Dose, High Dose) were significantly different from the control (Placebo).

  *  __Non-orthogonal comparisons__
    -  Similar to _orthogonal comparisons_ except the comparisons don't have to sum to zero.
    -  Example, you can compare across different levels, say Chunk1 is 'High Dose' only and Chunk2 is 'Placebo'

                  Dum_var1 (comp1), Dum_var2 (comp2), Product (comp1* comp2)
       Grp Placebo              -2,               -1,                     2
       Grp Low Dose              1,                0,                     0
       Grp High Dose             1,                1,                     1
       Total                     0                 0                      3
    
    -  The _p-values_ here are correlated so you'll need to be careful how to interpret the results.  Instead of .05 probability, you might want to have a more conservative level before accepting that a given comparison is statistically meaningful.

  *  __Polynomial Contrast (trend analysis)__ is a more complex way of looking at comparisons.  The different groups usually represent a different amount of a single common variable (e.g. amount of dosage for drug) and its effect
      -  __Linear Trend__ is where the group means increase proportionately (e.g. a positive linear trend is where the more of the drug we give, the more effect, looks like a linear line)
      -  __Quadratic Trend__ is where the the group means increase at the beginning, peak in the middle, then goes down (e.g. drug enhances performance if given a certain amount, then after a certain amount, the more you give the worse the performance); basically there's one direction change.  This requires at least 3 groups.
      -  __Cubic Trend__ is where the group means goes up, down, up (or vice versa); basically there's two direction changes.  This requires at least 4 groups.
      -  __Quartic Trend__ is where the group goes up, down, up, down (or vice versa); basically there's at least four direction changes. 

  2.  __post hoc comparisons (aka post hoc tets, data mining, exploring data)__
  Compare every group (like you're doing multiple _t-tests_) but using a stricter acceptance criterion so that the familywise error rate doesn't rise above the acceptable .05.  This is done when you have no specific hypothesis to test.
    -  _post hoc comparisons_ is kinda cheating since this consits of __pairwise comparisons__ that compare all different combinations of the treatment groups.  However, _pairwise comparisons_ control _familywise errors_ by correcting the level of significance for each test so that the overall _Type I error rate_ across all comparisons remains at .05.  This is accomplished in multiple ways:
        +  __Bonferroni correction__ is a trade-off for controlling the _familywise error rate_ with a loss of _statistical power_; this means that the probability of rejecting an effect that does actually exist is increased (_Type II error_).  For example, we do 10 tests, we use .005 as our criterion for significant (instead of .05).  Formula is `<insert formula>`
        +  __Holm's method__ computes the _p-values_ for all of the pairs of groups in your data, then order them from smallest to largest.  We start similar with the normal _Bonferroni correction_ for the first comparison, but then each subsequent comparison the _p-value_ gets bigger (i.e. less conservative).  This method is _stepped_, which means we continue as long as comparisons are significant.  If there's a non-significant comparison we stop and assume all remaining comparisons are also non-significant.
        +  __Benjamini-Hochberg__'s method doesn't focus on making _Type I error rate_ like the above methods (basically, if we make a _Type I_ error, it's not that bad) and instead focuses on __False Discovery Rate (FDR)__, which is the proportion of falsely rejected null hypothesis to the total number of rejected null hypothesis.  The _Benjamini-Hochberg_ tries to keep the _FDR_ under control instead of the _familywise error rate_.  Like _Holm's method_, you comput the _p-value_ for all pairs of groups in your data and order them similarly (smallest to largest).  The difference is that this method is _step-up_, which means instead of working down the talbe we work up.  We say the bottom value is non-significant and continue moving up the table until we see a significant comparison, then assume that all other comparisons are also significant.
    -  So which _post hoc procedure_ should you use?  There's these and numerous others that R doesn't do out of the box.  When deciding, consider:
        1.  Whether the test controls the _Type I error rate_
        2.  Whether the test controls the _Type II error rate_ (i.e. has good statistical power)
        3.  Whether the test is reliable when the test assumptions of _ANOVA_ have been violated
        *  _Bonferroni_ and __Tukey's Honest Significant Difference (HSD) tests (aka Tukey's range test, Tukey method__ both control the _Type I error rate_, but are conservative so they lack _statistical power_.  _Bonferroni_ has more power when the number of comparisons is small.  _Tukey_ is more powerful when testing large numbers of means.
        *  _Benjamini-Hochberg_ has more power than _Holm's_; _Holm's_ has more power than _Bonferroni_.  Just remember _Benjamini-Hochberg_ doesn't attempt to control _Type I errors_.
        *  __Warning__: The above _post-hoc comparisons_ perform badly when group sizes are unequal and when population variances are different.  If this is the case, look up _bootstrapping_ or _trimmed means_ and _M-estimators_ (both of which include a bootstrap).  Use _bootstrap_ to control _Type I errors_ and _M-estimators_ if you want more _statistical power_.

    *  Note: for some reason, the effect size _r^2_ specifically for _ANOVAs_ are called __eta squared__ and looks like `n^2`

### Comparing Several Means with ANCOVA (Analysis of Covariance GLM 2)
_ANCOVA_ is like _ANOVA_, but also includes __covariates__, which are one or more continuous variables that are not part of the main experimental manipulation, but have an influence on the outcome (aka dependent variable).  We include _covariates_ because of two reaons:
  1. To reduce within-group error variance: In _ANOVA_, if we can explain some of the 'unexplained' variance _(SSr)__ in terms of _covariates_, then we reduce the error variance, allowing us to more accurately assess the effect of the independent variable _(SSm)_ 
  2. Elimination of confounds: this means that an experiment has unmeasured variables that confound the results (i.e. variables other than the experimental manipulation that affect the outcome variable).  _ANCOVA_ removes the bias from the variables that influence the independent variable.
*  E.g. Say we look at the example with the effects of Viagra on the libido; the _covariates_ would be other things like medication (e.g. antidepressants).  _ANCOVA_ attempts to measure these continuous variables and include them in the regression model.
*  ANCOVA has the same assumptions as any linear model with these two additional considerations:
  1. independence of the covariate and treatment effect - Unexplained variance _(SSr)_ should only overlap with the _Variance explained by Covariate_ (and not with _Variance explained by the independent variable_ or else the effect is obscured).
  2. __homogeneity of regression slopes__ - the slopes of the different regression lines should all be equivalent (i.e. parallel among groups)  For example, if there's a positive relationship between the covariate and the outcome in one group, then we assume there's a positive relationship for all other groups
*  __Sum of Squares (Type I, II, III, IV)__
  -  Remember that order matters when evaluating
  -  __Type I Sum of Squares__ We put one predictor into the model first (it gets evaluated), then the second (then it gets evaluated), etc.  Use if the variables are completely independent of each other (unlikely), then _Type I SS_ can evaluate the true effect of each variable.  
  -  __Type II Sum of Squares__ Use if you're interested in main effects; it gives an accurate picture of a main effect because they're evaluated ignoring the effect of any interactions involving the main effect under consideration.  However, if an interaction is present, then _Type II_ can't evaluate main effects (because variance from the interaction term is attributed to them)
  -  __Type III Sum of Squares__ is usually the default; use this when sample sizes are unequal, however they work only when predictors are encoded with _orthogonal contrasts_ instead of a _non-orthogonal contrast_.
  -  __Type IV Sum of Squares__ is the same as _Type III_, but is designed for situations in which there's missing data.
*  __Interpreting the covariate__ - Draw a scatterplot of the covariate against the outcome (e.g. participant's libido as y, partner's libido as x).  If covariate is positive, then there's a positive relationship where the covariate increases, so does the outcome.  If covariate is negative, then there's a negative relationship where the covariate increases, the outcome decreases.
*  __Partial eta squared (aka partial n^2)__ is an effect size measure for _ANCOVA_ (kinda similar to _eta squared (n^2)_ in _ANOVA_ or _r^2_).  This differs from _eta squared_ in that it looks not at the proportion of total variance that a variable explains, but at the proportion of variance that a variable explains that is not explained by other variables in the analysis.


### Factorial ANOVA (Independent Factorial Design, Repeated Measures Factorial Design, Mixed Design)
_ANOVA_ and _ANCOVA_ looks at differences between groups with only a single independent variable (i.e. just one variable being manipulated).  _Factorial ANOVA_ looks at differences between groups with two or more independent variables.  When there are two or more independent variables, it is __factorial design__ (because sometimes variables are known as _factors_).  There are multiple types of this design including:
    -  __independent factorial design__ - there are several independent variables and each has been measured using different entities 
    -  __repeated-measures (related) factorial design__ - there are several independent variables measured, but the same entities have been used in all conditions
    -  __mixed design__ - several independent variables have been measured, some have been measured with different entities and some used the same entities

__ANOVA naming convention__ - The names of _ANOVA_s can seem confusing, but are easy to break down.  We're simply saying how many independent variables and how they were measured.  This means:
1.  The quantity of independent variables (e.g. one independent variable translates to 'one-way independent')
2.  Are the people being measured the same or different participants?
    - If the same participants, we say __repeated measures__
    - If different participants, we say __independent__
    - If there are two or more independent variables, then it's possible some variables use the same participants while others use different participants so we say __mixed__


## Independent Factorial Design ANOVA (GLM 3)
An example of _Factorial ANOVA_ using two independent variables is looking at the effects of alcohol on mate selection at nightclubs.  The hypothesis was that after alcohol has been consumed (the first independent variable), subjective perceptions of physical attractiveness would become more inaccurate.  Say we're also interested if this effect is different for men and women (this is the second independent variable).  We break groups into gender (male, female), drinks (none, 2 pints, 4 pints), and measured based off an independent assessment of attractiveness (say 1 to 100).
*  Calculations for a _Two-way ANOVA_ is very similar to a _One-way ANOVA_ with the exception that in a _Two-way ANOVA_ the variance that is explained by the experiment (_SSm_) is broken down into the following:
    -  _SSa (aka main effect of variable A)_ is the variance explained by Variable A
    -  _SSb (aka main effect of variable B)_ - is the variance explained by Variable B
    -  _SSa*b (aka the interaction effect)_ - is the variance explained by the interaction of Variable A and Variable B
*  For the _F-ratio_, each one of the above effects (_SSa_, _SSb_, and _SSa*b_) all have their own _F-ratio_ so that we can tell if each effect happened by chance or reflects an effect of our experimental manipulations
*  __interaction graphs__ can help you interpret and visualize significant interaction effects
*  Once again, we run _Levene's test_ to see if there are any significant differences between group variances (_homogeneity of variance_); for example, check if the variance in attractiveness differs across all different gender and alcohol group combinations.
*  For choosing _contrasts_, we need to define contrasts for all of the independent variables.  If we use _Type II sums of squares_, then we have to use an _orthogonal contrast_.  If we want to look at the _interaction effect_ (i.e. the effect of one independent variable at individual levels of the other independent variable), we use a technique called __simple effects analysis__
*  Like _ANOVA_, do a _post hoc test_ on the _main effects_(e.g. _SSa_, _SSb_) in order to see where the differences between groups are.  If you want to see the _interaction effect_ (e.g. _SSa*b_) then look at _contrasts_.


## Repeated-Measures Designs ANOVA (aka Within-Subjects ANOVA, ANOVA for correlated samples, GLM 4)
_Repeated measures_ is when the same entities participate in all conditions of the experiment.  A _Repeated-Measures ANOVA_ is like a regular _ANOVA_, but it violates the assumption that scores in different conditions are independent (scores are likely to be related because they're from the same people), which will cause the _F-test_ to lack accuracy.
*  Since the _F-test_ lacks accuracy, we have to make a different assumption called the __assumption of sphericity (aka circularity)__, which is an assumption about the structure of the covariance matrix; we assume the relationship between pairs of experimental conditions is similar (More precisely, the variances of the differences between treatment levels is about the same).  That means we calculate the differences between pairs of scores for all combinations of the treatment level.  We then calculate the variance of these differences.  As such, _sphericity_ is only an issue with three or more variables.
*  __compound symmetry__ is a stricter requirement than sphericity (if this is met, so is _sphericity_; if this is not met, you still need to check for _sphericity_).  This requirement is true when both the variances across conditions are equal (same as the _homogeneity of variance assumption_ in _between-group designs_) and the covariances between pairs of conditions are equal.
*  __Mauchly's test__ is a way to assess _sphericity_ and let you know if the variances of the differences between conditions are equal.  If _Mauchly's test_ is significant, then we have to be wary of the _F-ratio_, otherwise you're okay.  However, remember that this is also dependent on sample size; large samples can have small deviations from sphericity to be significant while in small samples large violations can be non-significant.
*  Violating _sphericity_ means a loss of statistical power and it also impacts _post hoc tests_.  If _sphericity_ is definitely not violated, use _Tukey's test_.  If _sphericity_ is violated, then using the _Bonferroni method_ is usually the most robust of the univariate techniques in terms of power and control of _Type I error rate_.
*  __epsilon__ is a descriptive statistic that indicates the degree to which _sphericity_ has been violated.  The formula is: `epsilon = 1/(k-1)` where `k` is the number of repeated-measures conditions.  If epsilon is closer to 1, the more homogeneous the variances of differences (and closer to the data being spherical).
*  If data violates the _assumption of sphericity_, then we can apply a correction factor that is applied to the degrees of freedom used to assess the _F-ratio_ (or we can not use the _F-ratio_).
    -  __Greenhouse-Geisser correction__ - Use if epsilon is less than .75 or if nothing is known about sphericity (otherwise the correction is too conservative and there's too many false null hypothesis that fail to be rejected).  Go look up how to do this.
    -  __Huynh-Feldt correction__ - Use if estimate is more than .75
    -  Use a different test other than the _F-ratio_ like using multivariate test statistics (_multivariate analysis_, _MANOVA_) because they don't depend on the _assumption of sphericity_.  There's a trade-off in power between _univariate_ and _multivariate tests_.  Go look up how to do this.
    -  Analyze the data as a _multilevel model_ so we can interpret the model coefficients without worrying about sphericity because dummy-coding our group variables ensures that we only compare two things at once (and sphericity is only an issue when comparing three or more means).  Not for the faint of heart (think about doing a _multivariate test_ first)
*  The calculations for _repeated-measures ANOVA_ can be seen as `SSt = SSb + SSw` where _SSt_ is the Total Variability, _SSb_ is the Between-Participant variability and _SSw_ is the Within-Participant Variability.  The main difference is that the _SSw_ ('Within-Participant Sum of Squares') is now broken down into:
    -  _SSw_ looks at the variation with an actual person, not within a group of people; some of this variation is explained by the effect of the experiment and some of it is just random fluctuation
    -  _SSm_ (Model Sum of Squares) is the effect of the experiment
    -  _SSr_ (Residual Sum of Squares) represents the error; the variation not explained by the experiment
*  Example of this is if we tested a group of participants (Person1, Person2, Person3, Person4, Person5, Person6) each eating multiple gross foods (NastyFoodA, NastyFoodB, NastyFoodC, NastyFoodD) and measuring how long it takes to retch.  The independent variable is the type of food eaten (e.g. NastyFoodA) since that's what we're manipulating, the dependent variable is the time to retch (since it depends on the food).  _SSw_ represents each participant's own tolerance level for nasty food.  _SSm_ is how much our manipulation (change in food) calculates the mean for each level of the independent variable (mean time to retch for say NastyFoodA, NastyFoodB, etc) and compares this to the overall mean of all foods.  _SSr_ is just the error.
*  To check _effect size_ for _repeated-measures designs_, we calculate __omega squared (w^2)__.  This formula is slightly different than in _one-way independent ANOVAs_ and it looks pretty scary so google it up.
*  __Factorial repeated-measures designs__ is just extending the _repeated-measures designs_ to include a second or more independent variable.  If there's two independent measures, then it's a _two-way repeated-measures ANOVA_.  If there's three independent measures, then it's a _three-way repeated-measures ANOVA_, etc.  Again we'll need to be careful about interaction effects of multiple independent variables.

## Mixed Designs ANOVA (GLM 5)
__Mixed Designs ANOVA__ is where you compare several means when there are two or more independent variables and at least one independent variable has been measured using the same participants (_repeated-measures_) and at least another independent variable has been measured using different participants (_independent design_).
*  You can explore the data similar to a _repeated-measures design ANOVA_ or as a _multilevel model_.  If you're using the _ANOVA_ approach, check for the _assumption of sphericity_, then choose what you want to _contrast_, compute the main model (might need to run a robust version of the test), then follow up with your _post hoc tests_.

### Non-parametric Tests

### Multivariate Analysis of Variance (MANOVA)

### Factor Analysis


<remember to continue here for more ANOVA notes>

### Comparing Categorical Variables
For continuous variables we measure using the means, but this is useless for categorical variables (since we'd assign numbers to categories and it would just depend on how many categories there were).  Instead, for categorical variables, we count the _frequency_ of the category to get a _contingency table_, which is a tabulation of the frequencies.  We use different algorithms depending on how many categorical variables there are (2 or more than 2).
*  An example of two categoricals is say training (if the cat was trained using either food or affect, but not both) and dance (if the cat ended up being able to dance)
*  __Pearson's chi-square statistic (aka chi-square test)__ is used to compare the relationship between two categorical variables.  This is given by the equation ` `, which is a variation on `deviation = change in (observed -model)^2`.  Note that this is only an approximation (which works really well for large samples), but if the sample size is too small, use _Fisher's exact test_, _likelihood ratio_, or _Yate's continuity correction_ to avoid making _Type I errors_.
    -  __Fisher's exact test__ is a way to compute the exact probability of the _chi-square statistic_, useful for when the sample sizes are too small.  Usually this is used on 2 * 2 contingency tables (two categorical variables each with two categorical options) and small sample size, but can be used on larger at the cost of really intensive calculations
    -  __likelihood ratio statistic__ is an alternative to the _Pearson's chi-square_ and is based on the _maximum-likelihood theory_.  The idea is that you collect some data and create a model for which the probability of obtaining the observed set of data is maximized, then you compare this model to the probability of obtaining those data under the null hypothesis.  You then compare observed frequencies with those predicted by the model.  This is roughly the same as _Pearson's chi-square_, but preferred when samples are small.  The equation is: ` `
    -  __Yate's continuity correction__ is used specifically for 2 * 2 contingency tables because for these cases _Pearson's chi-square_ tends to make _Type I_ errors.  Be careful though, this sometimes overcorrects and produces a _chi-square_ value that is too small .  The equation is: ` `
*  Assumptions of _chi-square test_ are:
    -  Independence of data - this means that each person, item, entity contributes to only one cell of the contingency table; you can't use this on a _repeated measures design_ (e.g. train cats with food to dance, then trained the same cats with affection to see if they would dance)
    -  Expected fequencies should be _greater than 5_; although in larger contingency tables, it's okay to have up to 20% of expected frequencies below 5, the result is loss of statistical power (which means the test might fail to detect a genuine effect).  If there isn't more than 5, collect more data of that particular category.
*  Interpreting the _chi-squared test_; if the _p-value_ is less than .05, then there is a significant relationship between your two variables
*  __odds ratio__ is a way to calculate _effect size_ for categorical data.  _odds ratio_ are good for 2 * 2 contingency tables and probably not useful for anything larger.  Example is odds of cat dancing after food = (number that had food and danced / number that had food but didn't dance), say 28/10 = 2.8.  Repeat for odds of dancing after affection (let's say that's .0421).  Then to calculate the _odds ratio_ = (odds dancing after food / odds dancing after affection); odds ratio = (2.8/.0421) =6.65  This means an annimal trained with food has 6.65 times higher chance to dance than being trained with affection.  
    -  If we're interested in finding out which particular factor contributed significantly in larger contingency tables, we can use the _standardized residuals_.

### Comparing Multiple Categorical Variables using Loglinear Analysis
__loglinear analysis__ is used when we want to look at more than two categorical variables.  Think of this as the _ANOVA_ for _categorical variables_ (where for every variable we have, we get a main effect but we also get interactions between variables).  For example, say we want three variables: Animal (dog or cat), Training (food as reward or affection as reward) and Dance (did they dance or not?)  Again, this can also be seen as a regression model.  Let's not get into the math, but basically categorical data can be expressed in the form of a linear model provided we use log values (thus loglinear analysis).  The idea is that we try to fit a simpler model without any substantial loss of predictive power through backward elimination (remove one at a time hierarchically).
*  For example, say we have the following interactions for these three variables: Animal (dog or cat), Training (food or reward), Dance (can they dance or not).  We take the one interaction involving all three variables (i.e. _highest-order interaction_) and remove it.  We look at whether the new model without this interaction and if the new model significantly changes the likelihood ratio statistic, then we stop here and say that we have a significant three way interaction.  If there is no change in the likelihood ratio statistic, then we move to a lower-order interaction. 
    -  Three main effects (Animal, Training, Dance)
    -  Three interactions involving two variables each (Animal * Training, Animal * Dance, Training * Dance)
    -  One interaction involving all three variables (Animal * Training * Dance)
*  Assumptions in loglinear analysis include:
    -  _Loglinear analysis_ is an extension of the _chi-square test_ so it has similar assumptions
    -  Each cell must be independent
    -  Expected frequencies should be large enough for a reliable analysis.  With more than two variables, it's okay to have up to 20% of cells with expected frequencies less than 5; but all cells must have expected frequencies greater than 1.  If this assumption is broken, try to collapse the data across one or the variables
*  Reporting results of a loglinear analysis - Example: The three-way loglinear analysis produced a final model that retained all effects.  The likelihood ratio of this model was this and p was that.  This indicated that the highest order interaction (Animal * Training * Dance) was significant.  To break down this effect, separate chi-square tests on the Training and Dance variables were performed separately for dogs and cats.

### Multilevel linear models
Sometimes data is hierarchical instead of at a single level.  This means that some variables are clustered or nested within other variables (i.e. some of our other analysis may be oversimplification).  Let's use this example data set:
    -  __Level 1 variable__ - Say we have a lot of children (this is the lowest level of the hierarchy)
    -  __Level 2 variable__ - Say these students are organized by classrooms (this means children are _nested_ within classes)
    -  Note: We can have additional levels (e.g. level 3 variable is the school of that classroom, level 4 is the school in that school district)
*  The idea is that children at different layers (say within the same classroom or the same school) are more similar to each other.  This means that each case is not entirely independent.  To solve for this, we use __intraclass correlation (ICC)__, which represents the proportion of the total variability in the outcome that is attributable to the child's classroom.  This means that if the classroom had a huge effect on the children, then the _ICC_ will be small.  Howerver, if the classroom had little effect on the children, then the _ICC_ will be large.  Simply, _ICC_ says if this hierarchical grouping had an effect on the outcome.
*  So why use a _multilevel linear model_?  The benefits include:
    -  We don't have to assume that the relationship between our covariate and our outcome is the same across the different groups that make up our predictor variable.
    -  We don't need to make the assumption of independence.
    -  It's okay to have missing data.  You don't have to correct and impute for missing data.


