---
title: "Advanced data preparation practical 1"
teaching: 0
exercises: 40
---



## Exercise 1



~~~
library(palmerpenguins)
penguins
~~~
{: .language-r}


> ## Exercise 1 tasks and solution
> Your tasks are as follows: 
>
> 1) Have a look at the data and can you see any issues you may have? after noticing the problem, its time to clean the data
>
> 2) Convert the "sex" label to 1 and 0s.
>
> 3) you have created 4 models for predicting the sex of penguins with different variables. the AIC values from you models are as follows model1:50, model2:48, model3:60, model4:53. Using a Alpha = 0.05, which models can be dismissed? 
> 
> 4) build a linear model using the penguin dataset base on sex prediction and use the drop1 method to find the optimal collection of variables.
> 
> 
>
> > ## Solution
> > 1) The issue is that we have some missing values and if we want to do any sort of linear regression they will need to be removed.
> > 1) penguins=na.omit(penguins)
> > 1) penguins
> > 
> > 2) either penguin$sex <- ifelse(penguin$sex == "male", 0, 1) or penguin$sex <- ifelse(penguin$sex == "female", 0, 1) 
> > 
> > 3) assuming to 3dp, model1: 0.368, model3:0.003, model4:0.082. We can therefore reject model3.
> > 3) What value would model4 need to get for us to reject it?
> >
> > 4) lm1 <- lm(sex ~ ., data = penguins)
> > 
> > 4) drop1(lm1, test = "F")  # So called 'type II' anova
> >
> > 4) species + island + bill_length_mm + bill_depth_mm + flipper_length_mm + body_mass_g + year
> >
> >
> >
> {: .solution}
{: .challenge}

{% include links.md %}
