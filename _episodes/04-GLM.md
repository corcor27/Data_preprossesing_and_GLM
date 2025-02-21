---
title: "Generalised linear models (GLM)"
teaching: 20
exercises: 0
questions:
- "What are GLMs?"
- "How do we assess such methods?"
- "what statistical tests can we use"
objectives:
- "Learn how to use GLMs."
- "Learn how to asses the quality of the fit of our model."
- "Learn what statistical test are relevant to GLM and how to apply them."
keypoints:
- "We can model linear data using GLM models."
- "We learned how to assess the quality of fit of our model to the data."
- "We learned to apply ANOVA to GLM and can interpret the results"
---

# What are Linear models

Linear models represent a continuous response variable as a function of one or more predictor variables. They are useful for understanding and predicting the behaviour of complex systems, as well as analysing experimental, financial, and biological data. 

In this course, we will briefly explore **Linear Regression**, a statistical method used to construct a linear model. This model defines the relationship between a dependent variable y (also known as the response) and one or more independent variables &chi;<sub>i</sub> (referred to as predictors). There our model takes the form of the following equation:

<img src="https://latex.codecogs.com/svg.image?&space;y=\beta_{0}&plus;\sum\beta_{i}\chi_{i}&plus;\epsilon_{i}" title=" y=\beta_{0}+\sum\beta_{i}\chi_{i}+\epsilon_{i}" />

where &beta; represents linear parameter estimates to be computed and &epsilon; represents the error terms that is assumed to be normally distributed. 

There are several types of linear regression:

* Simple linear regression: models using only one predictor
* Multiple linear regression: models using multiple predictors
* Multivariate linear regression: models for multiple response variables

For this course will refine our selves to a short introduction into simple regression, so lets have at how we would go about calculating a line of best fit with an example. 

## Linear models using the cars dataset

For this analysis, we will use the built-in cars dataset, which comes with R by default. This dataset is widely used for demonstrating linear regression in a straightforward and accessible way. You can access it by typing cars in your R console. The dataset contains 50 observations (rows) and 2 variables (columns): speed and dist. Let's print the dataset to see what it includes.

~~~
> head(cars)
~~~
{: .language-r}

>![graph of the test regression data](../fig/cars_printout.png)
{: .output}

Now before we move on to creating a simple linear regression model, its always go practise to analyse our dataset first such that we can fully understand the variables. Therefore lets conduct some graphical analysis. 

## Graphical analysis

The goal of this exercise is to build a simple regression model to predict Distance (dist) by identifying a statistically significant linear relationship with Speed (speed). Before diving into the syntax, let's first explore these variables graphically.

To better understand the behaviour of the predictor variable, we typically use the following visualisations:

* Scatter Plot: Helps visualise the linear relationship between the predictor and response variables.
* Box Plot: Identifies potential outliers in the predictor variable. Outliers can significantly impact predictions by influencing the slope and direction of the best-fit line.
* Density Plot: Shows the distribution of the predictor variable. Ideally, the distribution should be approximately normal (a bell-shaped curve) without significant skewness.

Now, let's create these plots to analyse the dataset.

### Scatter Plot

Scatter plots can help visualise any linear relationships between the dependent (response) variable and independent (predictor) variables. Ideally, if you are having multiple predictor variables, a scatter plot is drawn for each one of them against the response, along with the line of best as seen below.

~~~
> scatter.smooth(x=cars$speed, y=cars$dist, main="Dist ~ Speed")  # scatterplot
~~~
{: .language-r}

>![graph of the test regression data](../fig/speed_scatter.png)
{: .output}

The scatter plot along with the smoothing line above suggests a linearly increasing relationship between the ‘dist’ and ‘speed’ variables. This is a good thing, because, one of the underlying assumptions in linear regression is that the relationship between the response and predictor variables is linear and additive.

### Boxplot to check for outliers

Generally, any datapoint that lies outside the 1.5 * interquartile-range (1.5 * IQR) is considered an outlier, where, IQR is calculated as the distance between the 25th percentile and 75th percentile values for that variable.

~~~
> par(mfrow=c(1, 2))  # divide graph area in 2 columns
> boxplot(cars$speed, main="Speed", sub=paste("Outlier rows: ", boxplot.stats(cars$speed)$out))  # box plot for 'speed'
> boxplot(cars$dist, main="Distance", sub=paste("Outlier rows: ", boxplot.stats(cars$dist)$out))  # box plot for 'distance'
~~~
{: .language-r}

>![graph of the test regression data](../fig/speed_box_plots.png)
{: .output}

### Density plot – Check if the response variable is close to normality

Its a good idea to check if our data takes the form of a normal distribution, to make choosing to use linear regression applicable.

~~~
> install.packages("e1071")
> library(e1071)
> par(mfrow=c(1, 2))  # divide graph area in 2 columns
> plot(density(cars$speed), main="Density Plot: Speed", ylab="Frequency", sub=paste("Skewness:", round(e1071::skewness(cars$speed), 2)))  # density plot for 'speed'
> polygon(density(cars$speed), col="red")
> plot(density(cars$dist), main="Density Plot: Distance", ylab="Frequency", sub=paste("Skewness:", round(e1071::skewness(cars$dist), 2)))  # density plot for 'dist'
> polygon(density(cars$dist), col="red")
~~~
{: .language-r}

>![graph of the test regression data](../fig/speed_box_plots.png)
{: .output}

### Correlation

Correlation is a statistical measure that suggests the level of linear dependence between two variables, that occur in pair – just like what we have here in speed and dist. Correlation can take values between -1 to +1. If we observe for every instance where speed increases, the distance also increases along with it, then there is a high positive correlation between them and therefore the correlation between them will be closer to 1. The opposite is true for an inverse relationship, in which case, the correlation between the variables will be close to -1.

A value closer to 0 suggests a weak relationship between the variables. A low correlation (-0.2 < x < 0.2) probably suggests that much of variation of the response variable (Y) is unexplained by the predictor (X), in which case, we should probably look for better explanatory variables.

~~~
> cor(cars$speed, cars$dist)  # calculate correlation between speed and distance 
~~~
{: .language-r}

~~~
[1] 0.8068949
~~~
{: .output}

## Building a linear model

Now that we have seen the linear relationship pictorially in the scatter plot and by computing the correlation, lets see the syntax for building the linear model. The function used for building linear models is lm(). The lm() function takes in two main arguments, namely: 1. Formula 2. Data. The data is typically a data.frame and the formula is a object of class formula. But the most common convention is to write out the formula directly in place of the argument as written below.

# What are GLMs

A Generalised Linear Model (GLM) extends ordinary linear regression by allowing response variables to follow error distributions other than the normal (Gaussian) distribution. Essentially, a GLM is a linear model with a modified error distribution that more accurately represents the data-generating process and has found common use for analysing data examples such as count data or binary. For example, if your response variable consists of binary outcomes, such as successes and failures coded as 1s and 0s, these values do not follow a normal distribution, nor would their residuals exhibit a normal error distribution. In such cases, adjusting the underlying distribution in the model ensures a better fit for the data.


We now create a basic linear model for a given dataset. It would be valuable to assess the accuracy of this model. One way to achieve this is by computing the predicted y-values for each x-value in our original dataset and comparing them with the actual y-values. We can aggregate these individual discrepancies into a single comprehensive error metric by calculating the least squares. This involves squaring each difference, summing them all, dividing the sum by the total number of observations, and then taking the square root of the result. By squaring and subsequently taking the square root, we prevent negative errors from offsetting positive ones, thus providing us with an overall error metric to gauge the accuracy of our model.

~~~
> linearMod <- lm(dist ~ speed, data=cars)  # build linear regression model on full data
> print(linearMod)

~~~
{: .language-r}

~~~
Call:
lm(formula = dist ~ speed, data = cars)

Coefficients:
(Intercept)        speed
    -17.579        3.932
~~~
{: .output}

Now that we have built the linear model, we also have established the relationship between the predictor and response in the form of a mathematical formula for Distance (dist) as a function for speed. For the above output, you can notice the ‘Coefficients’ part having two components: Intercept: -17.579, speed: 3.932 These are also called the beta coefficients. In other words,
dist = Intercept + (&beta; ∗ speed)
=> dist = −17.579 + 3.932∗speed

## Linear regression diagnostics

Now the linear model is built and we have a formula that we can use to predict the dist value if a corresponding speed is known. Is this enough to actually use this model? NO! Before using a regression model, you have to ensure that it is statistically significant. How do you ensure this? Lets begin by printing the summary statistics for linearMod.

~~~
> summary(linearMod)  # model summary

~~~
{: .language-r}

~~~
Call:
lm(formula = dist ~ speed, data = cars)

Residuals:
    Min      1Q  Median      3Q     Max 
-29.069  -9.525  -2.272   9.215  43.201 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -17.5791     6.7584  -2.601   0.0123 *  
speed         3.9324     0.4155   9.464 1.49e-12 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 15.38 on 48 degrees of freedom
Multiple R-squared:  0.6511,	Adjusted R-squared:  0.6438 
F-statistic: 89.57 on 1 and 48 DF,  p-value: 1.49e-12
~~~
{: .output}

## The p Value: Checking for statistical significance

The summary statistics above tells us a number of things. One of them is the model p-Value (bottom last line) and the p-Value of individual predictor variables (extreme right column under ‘Coefficients’). The p-Values are very important because, We can consider a linear model to be statistically significant only when both these p-Values are less that the pre-determined statistical significance level, which is ideally 0.05. This is visually interpreted by the significance stars at the end of the row. The more the stars beside the variable’s p-Value, the more significant the variable.
Null and alternate hypothesis

When there is a p-value, there is a hull and alternative hypothesis associated with it. In Linear Regression, the Null Hypothesis is that the coefficients associated with the variables is equal to zero. The alternate hypothesis is that the coefficients are not equal to zero (i.e. there exists a relationship between the independent variable in question and the dependent variable).
t-value

We can interpret the t-value something like this. A larger t-value indicates that it is less likely that the coefficient is not equal to zero purely by chance. So, higher the t-value, the better.

Pr(>|t|) or p-value is the probability that you get a t-value as high or higher than the observed value when the Null Hypothesis (the β coefficient is equal to zero or that there is no relationship) is true. So if the Pr(>|t|) is low, the coefficients are significant (significantly different from zero). If the Pr(>|t|) is high, the coefficients are not significant.

What this means to us? when p Value is less than significance level (< 0.05), we can safely reject the null hypothesis that the co-efficient β of the predictor is zero. In our case, linearMod, both these p-Values are well below the 0.05 threshold, so we can conclude our model is indeed statistically significant.

It is absolutely important for the model to be statistically significant before we can go ahead and use it to predict (or estimate) the dependent variable, otherwise, the confidence in predicted values from that model reduces and may be construed as an event of chance.

## How to calculate the t Statistic and p-Values?

When the model co-efficients and standard error are known, the formula for calculating t Statistic and p-Value is as follows: 

<img src="https://latex.codecogs.com/svg.image?&space;t-Statistic=\frac{\beta-coefficient}{Std.Error}" title=" t-Statistic=\frac{\beta-coefficient}{Std.Error}" />

~~~
modelSummary <- summary(linearMod)  # capture model summary as an object
modelCoeffs <- modelSummary$coefficients  # model coefficients
beta.estimate <- modelCoeffs["speed", "Estimate"]  # get beta estimate for speed
std.error <- modelCoeffs["speed", "Std. Error"]  # get std.error for speed
t_value <- beta.estimate/std.error  # calc t statistic
p_value <- 2*pt(-abs(t_value), df=nrow(cars)-ncol(cars))  # calc p Value
f_statistic <- linearMod$fstatistic[1]  # fstatistic
f <- summary(linearMod)$fstatistic  # parameters for model p-value calc
model_p <- pf(f[1], f[2], f[3], lower=FALSE)

~~~
{: .language-r}

>![graph of the test regression data](../fig/speed_statistic.png)
{: .output}

## AIC and BIC

The Akaike’s information criterion - AIC (Akaike, 1974) and the Bayesian information criterion - BIC (Schwarz, 1978) are measures of the goodness of fit of an estimated statistical model and can also be used for model selection. Both criteria depend on the maximized value of the likelihood function L for the estimated model.

The AIC is defined as:


AIC = (−2) × ln(L) + (2×k)

where, k is the number of model parameters and the BIC is defined as:


BIC = (−2) × ln(L) + k × ln(n)

where, n is the sample size.

For model comparison, the model with the lowest AIC and BIC score is preferred.

~~~
> AIC(linearMod)  # AIC
> BIC(linearMod)  # BIC

~~~
{: .language-r}

~~~
[1] 419.1569
[1] 424.8929
~~~
{: .output}

## Predicting Linear Models

So far we have seen how to build a linear regression model using the whole dataset. If we build it that way, there is no way to tell how the model will perform with new data. So the preferred practice is to split your dataset into a 80:20 sample (training:test), then, build the model on the 80% sample and then use the model thus built to predict the dependent variable on test data.

Doing it this way, we will have the model predicted values for the 20% data (test) as well as the actual (from the original dataset). By calculating accuracy measures (like min_max accuracy) and error rates (MAPE or MSE), we can find out the prediction accuracy of the model. Now, lets see how to actually do this..
### Step 1: Create the training (development) and test (validation) data samples from original data.

~~~
> # Create Training and Test data -
> set.seed(100)  # setting seed to reproduce results of random sampling
> trainingRowIndex <- sample(1:nrow(cars), 0.8*nrow(cars))  # row indices for training data
> trainingData <- cars[trainingRowIndex, ]  # model training data
> testData  <- cars[-trainingRowIndex, ]   # test data

~~~
{: .language-r}

### Step 2: Develop the model on the training data and use it to predict the distance on test data

~~~
> # Build the model on training data -
> lmMod <- lm(dist ~ speed, data=trainingData)  # build the model
> distPred <- predict(lmMod, testData)  # predict distance

~~~
{: .language-r}

### Step 3: Review diagnostic measures.

~~~
> summary (lmMod)  # model summary
> AIC (lmMod)

~~~
{: .language-r}

From the model summary, the model p value and predictor’s p value are less than the significance level, so we know we have a statistically significant model. Also, the R-Sq and Adj R-Sq are comparative to the original model built on full data.





{% include links.md %}
