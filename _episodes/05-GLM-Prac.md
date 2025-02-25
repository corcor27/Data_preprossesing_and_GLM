---
title: "GLM practical 1"
teaching: 15
exercises: 30
---

## Exercise 1

Using the fishing data in the COUNT library, let’s model the relationship between total abundance (totabund) and mean depth (meandepth). Total abundance are counts, and we might hypothesise that abundances of fishes decreases with increasing depth.

~~~
install.packages("COUNT")
library(COUNT)
data(fishing)

~~~
{: .language-r}


> ## Exercise 1 tasks and solution
> Your tasks are as follows: 
>
> 1) compare different GLM distributions—Poisson, Binomial, and Gaussian—to determine which version of the model provides the best fit. (hint using AIC might be a quick method)
>
> 2) Using your best model plot the line of best fit to the data.
>
> > ## Solution
> > pois.glm <- glm(totabund ~ meandepth, data = fishing, family = poisson)
> > 
> > summary(pois.glm)
> >
> > AIC(pois.glm)
> > 
> > result for poisson: [1] 16754.46
> > 
> > binomial model not possible: Error in eval(family$initialize) : y values must be 0 <= y <= 1
> > 
> > gaus.glm <- glm(totabund ~ meandepth, data = fishing, family = gaussian)
> >
> > summary(gaus.glm)
> >
> > AIC(gaus.glm)
> >
> > result for gaussian: [1] 1954.792
> >
> > as gaussian AIC is smaller than poisson AIC, gaussian provides better fit.
> {: .solution}
{: .challenge}

## Exercise 2 (harder question)

Using the YERockfish data in the FSAdata library, let’s model the relationship between fish maturity (maturity) and length (length). Maturity is a binary response (immature or mature), and we might hypothesise that the probability of being mature increases with length. Be prepared this example will require some data cleaning!!!

~~~
> install.packages("FSAdata")
> library(FSAdata)
> data("YERockfish")

~~~

> ## Exercise 2 tasks and solution
> Your tasks are as follows: 
>
> 1) Identify what is wrong with you data.
> 
> 2) Clean your data and create a GLM using the binomial distribution.
>
> 3) Using your model plot the line of best fit to the data.
>
> > ## Solution
> > 
> > 1) So looking at our data we can see that we have both missing values and our target labels are not in the form of 0 and 1.
> > 
> > 2)
> >
> > YERockfish2 <- na.omit(YERockfish) # remove missing values
> > 
> > YERockfish2$maturity2 <- ifelse(YERockfish2$maturity == "Immature", 0, 1) #convert Immature to 0 and mature to 1 and call the column maturity2
> > 
> > binom.glm <- glm(maturity2 ~ length, data = YERockfish2, family = binomial)
> > 
> > summary(binom.glm)
> >
> > AIC(binom.glm)
> >
> {: .solution}
{: .challenge}

{% include links.md %}
