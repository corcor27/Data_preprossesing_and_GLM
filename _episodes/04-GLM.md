---
title: "Linear models (LM)"
teaching: 20
exercises: 0
questions:
- "What are LMs?"
- "How do we assess such methods?"
- "what statistical tests can we use"
objectives:
- "Learn how to use LMs."
- "Learn how to asses the quality of the fit of our model."
- "Learn what statistical test are relevant to LM and how to apply them."
keypoints:
- "We can model linear data using LM models."
- "We learned how to assess the quality of fit of our model to the data."
- "We learned to apply ANOVA to LM and can interpret the results"
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
> cars
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

>![graph of the test regression data](../fig/speed_density_plot.png)
{: .output}

### Correlation

Correlation is a statistical measure that indicates the degree of linear dependence between two paired variables, such as speed and dist in our dataset. It ranges from -1 to +1:

* A correlation close to +1 suggests a strong positive relationship, meaning that as speed increases, dist also tends to increase.
* A correlation close to -1 indicates a strong negative relationship, where an increase in speed corresponds to a decrease in dist.
* A correlation near 0 implies a weak or no linear relationship between the variables.

If the correlation is low (between -0.2 and 0.2), it suggests that the predictor (X) explains little of the variation in the response variable (Y). In such cases, we may need to explore alternative explanatory variables for better predictions.

~~~
> cor(cars$speed, cars$dist)  # calculate correlation between speed and distance 
~~~
{: .language-r}

~~~
[1] 0.8068949
~~~
{: .output}

## Building a linear model

Now that we've visualized the linear relationship using a scatter plot and confirmed it by calculating the correlation, let's move on to building the linear model.

In R, we use the lm() function to create linear models. This function requires two main arguments:

* Formula – Defines the relationship between the response and predictor variables.
* Data – A dataset (typically a data.frame) containing the variables.

Although the formula can be stored as an object of class formula, it is most commonly written directly within the function call, as shown in the example below.

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

Now that we've built the linear model, we have defined the relationship between the predictor (speed) and the response (dist) in the form of a mathematical equation.

From the model output, you’ll notice the Coefficients section, which consists of two components:

* Intercept: -17.579
* Speed: 3.932

These values are known as beta coefficients, and they define the equation for Distance (dist) as a function of Speed (speed):
dist = Intercept + (&beta; ∗ speed)
Substituting the values:
=> dist = −17.579 + 3.932∗speed

## Linear regression diagnostics

Now that we've built the linear model and derived a formula to predict dist based on a given speed, can we immediately start using it? Not yet!

Before applying a regression model, we must first verify its statistical significance. But how do we do that?

A good starting point is to examine the summary statistics of our model. Let’s print the summary of linearMod to evaluate its significance and performance.

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

The summary statistics provide valuable insights into our model's reliability. Two key aspects to examine are:

* The model’s overall p-value (found in the last line of the output).
* The p-values of individual predictor variables (located in the rightmost column under "Coefficients").

### Understanding p-values

P-values are crucial because a linear model is considered statistically significant only if both the model’s p-value and the predictor’s p-value are less than 0.05 (the standard significance level). This is visually represented by significance stars next to each variable—more stars indicate higher significance.

###Null and Alternative Hypotheses

Every p-value corresponds to a hypothesis test:

* Null Hypothesis (H₀): The coefficient of the predictor is zero, meaning no relationship exists between the independent and dependent variable.
* Alternative Hypothesis (H₁): The coefficient is not zero, indicating a significant relationship.

### Interpreting the t-value

The t-value measures how strongly a predictor variable influences the response variable:

* A larger t-value suggests a lower probability that the coefficient is zero by chance, meaning the predictor is more significant.
* The p-value (Pr(>|t|)) tells us the probability of obtaining such a high t-value under the null hypothesis. A low p-value (< 0.05) means the coefficient is significantly different from zero.

### What This Means for Our Model

In our case, both the model’s and the predictor’s p-values in linearMod are well below 0.05. This allows us to reject the null hypothesis and conclude that the predictor (speed) has a statistically significant relationship with dist.

Ensuring statistical significance is critical before using a model for predictions—without it, our predictions may lack reliability and could simply be due to chance.

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

Up to this point, we've built a linear regression model using the entire dataset. However, this approach doesn’t allow us to assess how well the model will perform on new data.

A better practice is to split the dataset into training (80%) and test (20%) subsets. The model is trained on the 80% sample, and then its performance is evaluated by predicting the dependent variable on the test data.

By doing this, we obtain both:

* Predicted values for the test data.
* Actual values from the original dataset.

We can then measure the model’s accuracy using metrics like Min-Max Accuracy and error rates such as MAPE (Mean Absolute Percentage Error) or MSE (Mean Squared Error). These help determine how well the model generalises to new data.

Now, let's walk through the process of implementing this approach.
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

~~~

Call:
lm(formula = dist ~ speed, data = trainingData)

Residuals:
Min      1Q  Median      3Q     Max 
-23.350 -10.771  -2.137   9.255  42.231 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  -22.657      7.999  -2.833  0.00735 ** 
speed          4.316      0.487   8.863 8.73e-11 ***

Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 15.84 on 38 degrees of freedom
Multiple R-squared:  0.674,  Adjusted R-squared:  0.6654 
F-statistic: 78.56 on 1 and 38 DF,  p-value: 8.734e-11

[1] 338.4489
~~~
{: .output}

From the model summary, the model p value and predictor’s p value are less than the significance level, so we know we have a statistically significant model. Also, the R-Sq and Adj R-Sq are comparative to the original model built on full data.

### Step 4: Lets explore what our line of best fit looks like

~~~
> plot(cars$speed, cars$dist, pch=21, main="Cars speed against distance", xlab="speed", ylab="distance")
> abline(lm(dist ~ speed, data=cars)$coefficients, col="red")
~~~
{: .language-r}

>![graph of the test regression data](../fig/speed_chart.png)
{: .output}

{% include links.md %}
