---
title: "GLM practical 1"
teaching: 0
exercises: 30
---


## Preprocess the dataset

Any easy way to calculate our intercepts is to use least squares fit. 
~~~
> lsfit(iris$Petal.Length, iris$Petal.Width)$coefficients # find linear fit intercepts
~~~
{: .language-r}
~~~
Intercept X
-0.3630755 0.4157554 .4
~~~
{: .output}

So now we have our intercepts, lets plot our line of best fit to our data.
~~~
> plot(iris$Petal.Length, iris$Petal.Width, pch=21, bg=c("red","green3","blue")[unclass(iris$Species)], main="Edgar Anderson's Iris Data", xlab="Petal length", ylab="Petal width")
> abline(lsfit(iris$Petal.Length, iris$Petal.Width)$coefficients, col="black") ### plot the clusters with linear line.
> legend("top",levels(iris$Species), pch = 21, col = c("red","green3","blue")) 
~~~
{: .language-r}

>![graph of the test regression data](../fig/petal_l_w.png)
{: .output}

So lets now have ago at building a linear model instead using "lm"
~~~
> lm_fit <- lm(Petal.Width ~ Petal.Length, data=iris) ## create linear model
> lm_fit$coefficients
~~~
{: .language-r}

~~~
(Intercept) Petal.Length
-0.3630755 0.4157554 
~~~
{: .output}

Again lets plot our linear model

~~~
> plot(iris$Petal.Length, iris$Petal.Width, pch=21, bg=c("red","green3","blue")[unclass(iris$Species)], main="Edgar Anderson's Iris Data", xlab="Petal length", ylab="Petal width")
> abline(lm(Petal.Width ~ Petal.Length, data=iris)$coefficients, col="black") ## plot linear model
> legend("top",levels(iris$Species), pch = 21, col = c("red","green3","blue")) 
~~~
{: .language-r}

>![graph of the test regression data](../fig/petal_l_w.png)
{: .output}

We can also look at how well our linear model fits the data by examining the p values and also have our model predict values for Petal width.

~~~
> summary(lm(Petal.Width ~ Petal.Length, data=iris)) 
> newdata = data.frame(Petal.Length=c(2,3,5)) ##create dataframe of features to predict
> predict(lm_fit, newdata) ## predict linear model
~~~
{: .language-r}

~~~
Call:
lm(formula = Petal.Width ~ Petal.Length, data = iris)

Residuals:
     Min       1Q   Median       3Q      Max 
-0.56515 -0.12358 -0.01898  0.13288  0.64272 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)  -0.363076   0.039762  -9.131  4.7e-16 ***
Petal.Length  0.415755   0.009582  43.387  < 2e-16 ***

Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.2065 on 148 degrees of freedom
Multiple R-squared:  0.9271,	Adjusted R-squared:  0.9266 
F-statistic:  1882 on 1 and 148 DF,  p-value: < 2.2e-16

Prediction Results

        1         2         3 
0.4684353 0.8841907 1.7157016 
~~~
{: .output}

> ## Try different features
>
> Have ago at using the same code and trying with sepal instead of petal, or any combination.
>
{: .challenge}

{% include links.md %}
