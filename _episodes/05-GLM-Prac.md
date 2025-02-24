---
title: "GLM practical 1"
teaching: 15
exercises: 30
---

Exercise 1 (easier)

Using the fishing data in the COUNT library, let’s model the relationship between total abundance (totabund) and mean depth (meandepth). Total abundance are counts, and we might hypothesise that abundances of fishes decreases with increasing depth.

~~~
> install.packages("COUNT")
> library(COUNT)
> data(fishing)

~~~
{: .language-r}


> ## Exercise 1 solution
> Your tasks are as follows: 
> 1) compare different GLM distributions—Poisson, Binomial, and Gaussian—to determine which version of the model provides the best fit. (hint using AIC might be a quick method)
> 2) Using your best model plot the line of best fit to the data.
> > ## Solution
> > pois.glm <- glm(totabund ~ meandepth, data = fishing, family = poisson)
> > summary(pois.glm)
> > AIC(pois.glm)
> > result for poisson: [1] 16754.46
> > 
> > binomial model not possible: Error in eval(family$initialize) : y values must be 0 <= y <= 1
> > 
> > gaus.glm <- glm(totabund ~ meandepth, data = fishing, family = gaussian)
> > summary(gaus.glm)
> > AIC(gaus.glm)
> > result for gaussian: [1] 1954.792
> > as gaussian AIC is smaller than poisson AIC, gaussian provides better fit.
> {: .solution}
{: .challenge}

Exercise 2 (harder)

Using the YERockfish data in the FSAdata library, let’s model the relationship between fish maturity (maturity) and length (length). Maturity is a binary response (immature or mature), and we might hypothesise that the probability of being mature increases with length. Be prepared this example will require some data cleaning!!!

~~~
> install.packages("FSAdata")
> library(FSAdata)
> data("YERockfish")

~~~
Your tasks are as follows: 

* Clean your data and create a GLM using the binomial distribution.
* Using your model plot the line of best fit to the data.

> ## Using environment variables to change program behaviour
>
> Set a shell variable `TIME_STYLE` to have a value of `iso` and check this
> value using the `echo` command.
>
> Now, run the command `ls` with the option `-l` (which gives a long format).
>
> `export` the variable and rerun the `ls -l` command. Do you notice any
> difference?
>
> > ## Solution
> >
> > The `TIME_STYLE` variable is not _seen_ by `ls` until is exported, at which
> > point it is used by `ls` to decide what date format to use when presenting
> > the timestamp of files.
> >
> {: .solution}
{: .challenge}

{% include links.md %}
