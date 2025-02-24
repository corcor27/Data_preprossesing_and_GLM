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

# What are GLMs

A Generalised Linear Model (GLM) extends ordinary linear regression by allowing response variables to follow error distributions other than the normal (Gaussian) distribution. Essentially, a GLM is a linear model with a modified error distribution that more accurately represents the data-generating process and has found common use for analysing data examples such as count data or binary. For example, if your response variable consists of binary outcomes, such as successes and failures coded as 1s and 0s, these values do not follow a normal distribution, nor would their residuals exhibit a normal error distribution. In such cases, adjusting the underlying distribution in the model ensures a better fit for the data.

We now create a basic linear model for a given dataset. It would be valuable to assess the accuracy of this model. One way to achieve this is by computing the predicted y-values for each x-value in our original dataset and comparing them with the actual y-values. We can aggregate these individual discrepancies into a single comprehensive error metric by calculating the least squares. This involves squaring each difference, summing them all, dividing the sum by the total number of observations, and then taking the square root of the result. By squaring and subsequently taking the square root, we prevent negative errors from offsetting positive ones, thus providing us with an overall error metric to gauge the accuracy of our model.

## GLM using mtcars dataset

We will use the “mtcars” dataset in R to illustrate the use of generalised linear models. This dataset includes data on different car models, including mpg, horsepower (hp), and weight. (wt). The response variable will be “mpg,” and the predictor factors will be “hp” and “wt.”

~~~
> mtcars
> head(mtcars)
~~~
{: .language-r}

Now as we did before with the linear version, its a good idea to analyses our dataset first so lets visualise our data. But before we do we need to first combine the two column “hp” and “wt" by adding them together.

~~~
> mtcars$hpwt <- mtcars$hp + mtcars$wt
~~~
{: .language-r}

## Graphical analysis

### Scatter Plot

Scatter plots can help visualise any linear relationships between the dependent (response) variable and independent (predictor) variables. Ideally, if you are having multiple predictor variables, a scatter plot is drawn for each one of them against the response, along with the line of best as seen below.

~~~
> scatter.smooth(x=mtcars$mpg, y=mtcars$hpwt, main="Mpg ~ hpwt")
~~~
{: .language-r}

>![graph of the test regression data](../fig/mt_scatter.png)
{: .output}


### Boxplot to check for outliers

Generally, any datapoint that lies outside the 1.5 * interquartile-range (1.5 * IQR) is considered an outlier, where, IQR is calculated as the distance between the 25th percentile and 75th percentile values for that variable.

~~~
> par(mfrow=c(1, 2))  # divide graph area in 2 columns
> boxplot(mtcars$mpg, main="Mpg", sub=paste("Outlier rows: ", boxplot.stats(mtcars$mpg)$out))  # box plot for 'mpg'
> boxplot(mtcars$hpwt, main="hpwt", sub=paste("Outlier rows: ", boxplot.stats(mtcars$hpwt)$out))  # box plot for 'hpwt'
~~~
{: .language-r}

>![graph of the test regression data](../fig/mt_boxplots.png)
{: .output}

### Density plot – Check if the response variable is close to normality

Its a good idea to check what form our data is in, to make choosing to use GLM applicable.

~~~
> library(e1071)
> par(mfrow=c(1, 2))  # divide graph area in 2 columns
> plot(density(mtcars$mpg), main="Density Plot: mpg", ylab="Frequency", sub=paste("Skewness:", round(e1071::skewness(mtcars$mpg), 2)))  # density plot for 'mpg'
> polygon(density(mtcars$mpg), col="red")
> plot(density(mtcars$hpwt), main="Density Plot: hpwt", ylab="Frequency", sub=paste("Skewness:", round(e1071::skewness(mtcars$hpwt), 2)))  # density plot for 'hpwt'
> polygon(density(mtcars$hpwt), col="red")
~~~
{: .language-r}

>![graph of the test regression data](../fig/mt_density.png)
{: .output}

## Building the model

The Gaussian family is used in this example, which implies that the response variable has a normal distribution. The glm() function yields an object of class “glm” containing model information such as coefficients and deviance.

~~~
> model <- glm(mpg ~ hp + wt, data = mtcars, family = gaussian)
> summary(model)
~~~
{: .language-r}


### Why Gaussian family?

The model may be clearly understood in terms of the mean and variance of the response variable, which is one benefit of employing the Gaussian family. Additionally, the model can be fitted using the well-known and popular statistical technique known as maximum likelihood estimation.

~~~
> summary(model)
~~~
{: .language-r}

~~~
Call:
glm(formula = mpg ~ hp + wt, family = gaussian, data = mtcars)

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 37.22727    1.59879  23.285  < 2e-16 ***
hp          -0.03177    0.00903  -3.519  0.00145 ** 
wt          -3.87783    0.63273  -6.129 1.12e-06 ***

Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for Gaussian family taken to be 6.725785)

    Null deviance: 1126.05  on 31  degrees of freedom
Residual deviance:  195.05  on 29  degrees of freedom
AIC: 156.65

Number of Fisher Scoring iterations: 2
~~~
{: .output}

A one-unit hp increase predicts a 0.03177 mpg decrease, while wt increase predicts a 3.87783 mpg decrease. Significance: All coefficients (intercept, hp, wt) are statistically significant, ensuring reliability. 

* Fit: Low residual deviance (195.05) versus null deviance (1126.05) and AIC (156.65) indicate a well-fitting model. 
* Dispersion (6.725785) measures mpg variability, essential for assessing prediction precision. Practical: The model aids understanding and prediction of fuel efficiency, valuable for automotive design and environmental considerations.

## Visualise the model

~~~
> plot(model, which = 1) # Plot the residual vs fitted values
> plot(model, which = 2) # Plot the Q-Q plot of residuals
~~~
{: .language-r}

>![graph of the test regression data](../fig/mtcars_plots.png)
{: .output}

After creating an extended linear model, we must evaluate its fit to the data. This can be accomplished with the help of diagnostic graphs such as the residual plot and the Q-Q plot. The output is shown above.

The residual plot displays the residuals (differences between measured and predicted values) plotted against the fitted values. (i.e. the predicted values). We want to see a random scatter of residuals around zero, which indicates that the model is capturing the data trends.
The residuals Q-Q plot displays the residuals plotted against the anticipated values if they were normally distributed. The points should follow a straight line, showing that the residuals are normally distributed.

## Using ANOVA to compare two models

We will fit two glm models to the data:

* A simple model with fewer predictors.
* A complex model with more predictors.

We can now use the anova() function to compare the two models using a likelihood ratio test. 
~~~
> simple_model <- glm(mpg ~ hp, data = mtcars, family = gaussian)
> complex_model <- glm(mpg ~ hp + wt, data = mtcars, family = gaussian)
~~~
{: .language-r}

~~~
Model 1: mpg ~ hp
Model 2: mpg ~ hp + wt
  Resid. Df Resid. Dev Df Deviance Pr(>Chi)    
1        30     447.67                         
2        29     195.05  1   252.63 8.86e-10 ***
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
~~~
{: .output}

The anova() function performs the test, and the argument test = "Chisq" specifies that a chi-squared test should be used. The result will include the degrees of freedom (Df), deviance, and the p-value of the test.

~~~
> p_value <- model_comparison$`Pr(>Chi)`[2]   # Second row for comparison between models
> print(p_value)
~~~
{: .language-r}

~~~
[1] 8.860267e-10
~~~
{: .output}

Here, the Pr(>Chi) column holds the p-values, and we select the second row ([2]), which corresponds to the comparison between the two models. The resulting p_value will give the significance of adding the additional predictor(s) in the complex model.

* High p-value (p > 0.05): The simpler model is sufficient. There is no evidence that the complex model is a better fit.
* Low p-value (p < 0.05): The complex model provides a significantly better fit, and the additional predictor(s) improve the model.

For example, if the p-value is 0.03, this would indicate that the more complex model is significantly better than the simpler model at the 5% significance level.

{% include links.md %}
