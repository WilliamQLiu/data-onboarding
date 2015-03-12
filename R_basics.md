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

### Statistics

## Check for normal a normal distribution

__Shapiro-Wilk test__ used as a way to test for _normality (aka normal distribution)_

    shapiro.test(variable)

__Q-Q plot (aka Quantile-Quantile plot)__ used as a way to test for _normality (aka normal distribution)_ 

    qplot(sample = rexam$exam, stat="qq")

__Levene's Test__ tests the null hypothesis that the variances in different groups are equal (i.e. the difference between the variances is zero).  This is from the _car_ package.  If _Levene's Test_ is significant (i.e. p<=.05), then we can conclude the null hypothesis is incorrect and the _assumption of homogeneity of variances_ is violated.  If _Levene's Test_ is non-significant (i.e. p>.05) then the variances are roughly equal and the assumption holds true.


    leveneTest(outcome_variable, group, center = median/mean)
    # where the outcome_variable is what we want to test the variances
    # group variable is a factor
    # center can be median or mean


