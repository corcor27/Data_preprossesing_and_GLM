---
title: "Advanced data preparation"
teaching: 20
exercises: 0
questions:
- How do we prepare data form more complex methods?
- How do we tested linear models?
objectives:
- "To understand how to edit data frames, including find problematic values and edit/removing them"
- "To understand what data is ready for use with GLMs"
- "How to test different GLMs"
- "How to find the best collection of variables for GLM"

keypoints:
- "Edit dataframes"
- "types of data is ready for GLM"
- "method to test and find the best models

---

## Dataframes

Data Frames are data displayed in a format as a table and are a powerful tool when it comes to performing more advanced computational methods. 

### Creating a Dataframe

Data Frames can have different types of data inside it. While the first column can be character, the second and third can be numeric or logical. However, each column should have the same type of data.

~~~
Data_Frame <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45))

# Print the data frame
Data_Frame 
~~~
{: .language-r}

~~~
  Training Pulse Duration
1 Strength   100       60
2  Stamina   150       30
3    Other   120       45
~~~
{: .output}

### Summarising dataframes
Use the summary() function to summarize the data from a Data Frame:
~~~
Data_Frame <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45))

Data_Frame

summary(Data_Frame) 
~~~
{: .language-r}

~~~
  Training             Pulse          Duration   
 Length:3           Min.   :100.0   Min.   :30.0  
 Class :character   1st Qu.:110.0   1st Qu.:37.5  
 Mode  :character   Median :120.0   Median :45.0  
                    Mean   :123.3   Mean   :45.0  
                    3rd Qu.:135.0   3rd Qu.:52.5  
                    Max.   :150.0   Max.   :60.0 
~~~
{: .output}

### Accessing items in a dataframe
We can use single brackets [ ], double brackets [[ ]] or $ to access columns from a data frame:
~~~
Data_Frame <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45))

Data_Frame[1]

Data_Frame[["Training"]]

Data_Frame$Training 
~~~
{: .language-r}

~~~
Data_Frame[1]
  Training
1 Strength
2  Stamina
3    Other

Data_Frame[["Training"]]
[1] "Strength" "Stamina"  "Other"   

Data_Frame$Training 
[1] "Strength" "Stamina"  "Other"   
~~~
{: .output}

### Adding rows
Use the rbind() function to add new rows in a Data Frame:
~~~
Data_Frame <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45))

# Add a new row
New_row_DF <- rbind(Data_Frame, c("Strength", 110, 110))

# Print the new row
New_row_DF 
~~~
{: .language-r}

~~~
  Training Pulse Duration
1 Strength   100       60
2  Stamina   150       30
3    Other   120       45
4 Strength   110      110
~~~
{: .output}

### Adding columns
Use the cbind() function to add new columns in a Data Frame:
~~~
Data_Frame <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45)) 
New_col_DF <- cbind(Data_Frame, Steps = c(1000, 6000, 2000))# Add a new column
New_col_DF # Print the new column
~~~
{: .language-r}

~~~
Training Pulse Duration Steps
1 Strength   100       60  1000
2  Stamina   150       30  6000
3    Other   120       45  2000
~~~
{: .output}

### Remove Rows and Columns

Use the c() function to remove rows and columns in a Data Frame:
~~~
Data_Frame <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45))

Data_Frame_New <- Data_Frame[-c(1), -c(1)]# Remove the first row and column

Data_Frame_New # Print the new data frame
~~~
{: .language-r}

~~~
Training Pulse Duration Steps
  Pulse Duration
2   150       30
3   120       45
~~~
{: .output}

### Amount of Rows, Columns and length of dataframe

Use the dim() function to find the amount of rows and columns in a Data Frame:

~~~
Data_Frame <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45))

dim(Data_Frame)
ncol(Data_Frame)
nrow(Data_Frame)
length(Data_Frame)
~~~
{: .language-r}

~~~
> dim(Data_Frame)
[1] 3 3
> ncol(Data_Frame)
[1] 3
> nrow(Data_Frame)
[1] 3
> length(Data_Frame)
[1] 3
~~~
{: .output}

### Combining Data Frames

Use the rbind() function to combine two or more data frames in R vertically:

~~~
Data_Frame1 <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45))

Data_Frame2 <- data.frame (
Training = c("Stamina", "Stamina", "Strength"),
Pulse = c(140, 150, 160),
Duration = c(30, 30, 20))

New_Data_Frame <- rbind(Data_Frame1, Data_Frame2)
New_Data_Frame 
~~~
{: .language-r}

~~~
  Training Pulse Duration
1 Strength   100       60
2  Stamina   150       30
3    Other   120       45
4  Stamina   140       30
5  Stamina   150       30
6 Strength   160       20
~~~
{: .output}

Use the rbind() function to combine two or more data frames in R vertically:

~~~
Data_Frame3 <- data.frame (
Training = c("Strength", "Stamina", "Other"),
Pulse = c(100, 150, 120),
Duration = c(60, 30, 45))

Data_Frame4 <- data.frame (
Steps = c(3000, 6000, 2000),
Calories = c(300, 400, 300))

New_Data_Frame1 <- cbind(Data_Frame3, Data_Frame4)
New_Data_Frame1 
~~~
{: .language-r}

~~~
  Training Pulse Duration Steps Calories
1 Strength   100       60  3000      300
2  Stamina   150       30  6000      400
3    Other   120       45  2000      300
~~~
{: .output}

## Remove rows with missing values

What are missing values?

Missing values are the data points that are absent for a specific variable in a dataset. It can be represented in various ways such as Blank spaces, null values, or any special symbols like"NA".Because of these various reasons missing values can occur, such as data entry errors, malfunction in equipment...etc.Dealing with missing data is a crucial step in data analysis. Some of the methods are.

* na.omit()
* complete.cases()

### Removing rows with na.omit

~~~
df1= data.frame(  
A1 = c(NA, 10, NA, 7, 8, 11,20),
A2 = c("A", 9, 3, "B", "C", "D","E"),
A3 = c(1, 0, NA, 1, 1, NA,3))
print(df1) #printing the dataframe

print("After removing the NA values ")
result=na.omit(df1)
print(result)
~~~
{: .language-r}

~~~
> print(df1)
  A1 A2 A3
1 NA  A  1
2 10  9  0
3 NA  3 NA
4  7  B  1
5  8  C  1
6 11  D NA
7 20  E  3
> 
> print("After removing the NA values ")
[1] "After removing the NA values "

  A1 A2 A3
2 10  9  0
4  7  B  1
5  8  C  1
7 20  E  3
~~~
{: .output}

### Remove rows with missing values using complete.cases()

~~~
df1 <- data.frame(
A1 = c(NA, 10, NA, 7, 8, 11,20),
A2 = c("A", 9, 3, "B", "C", "D","E"),
A3 = c(1, 0, NA, 1, 1, NA,3))
print(df1)#printing the dataframe

print("After removing the NA values ")
result=df1[complete.cases(df1),]
print(result)
~~~
{: .language-r}

~~~
> #printing the dataframe
> print(df1)
  A1 A2 A3
1 NA  A  1
2 10  9  0
3 NA  3 NA
4  7  B  1
5  8  C  1
6 11  D NA
7 20  E  3
> 
> print("After removing the NA values ")
[1] "After removing the NA values "

  A1 A2 A3
2 10  9  0
4  7  B  1
5  8  C  1
7 20  E  3
~~~
{: .output}

## Identify and Remove Duplicate Data

### Identifying Duplicate Data in vector

~~~

vector_data <- c(1, 2, 3, 4, 4, 5) # Create a sample vector with duplicate elements
duplicated(vector_data) # Identify duplicate elements
sum(duplicated(vector_data)) # count of duplicated data

~~~
{: .language-r}

~~~
[1] FALSE FALSE FALSE FALSE  TRUE FALSE
[1] 1
~~~
{: .output}

### Removing Duplicate Data in vector

~~~

vector_data <- c(1, 2, 3, 4, 4, 5)
unique(vector_data)# Remove duplicate elements

~~~
{: .language-r}

~~~
[1] 1 2 3 4 5
~~~
{: .output}

### Identifying Duplicate Data in a data frame

~~~

student_result=data.frame(name=c("Ram","Geeta","John",
"Paul","Cassie","Geeta","Paul"),maths=c(7,8,8,9,10,8,9),
science=c(5,7,6,8,9,7,8),
history=c(7,7,7,7,7,7,7))
 

student_result # Printing data
duplicated(student_result)
sum(duplicated(student_result))

~~~
{: .language-r}

~~~
    name maths science history
1    Ram     7       5       7
2  Geeta     8       7       7
3   John     8       6       7
4   Paul     9       8       7
5 Cassie    10       9       7
6  Geeta     8       7       7
7   Paul     9       8       7
[1] FALSE FALSE FALSE FALSE FALSE  TRUE  TRUE
[1] 2
~~~
{: .output}

### Removing Duplicate Data in a data frame

~~~
student_result=data.frame(name=c("Ram","Geeta","John","Paul",
"Cassie","Geeta","Paul"),
maths=c(7,8,8,9,10,8,9),
science=c(5,7,6,8,9,7,8),
history=c(7,7,7,7,7,7,7))
 

student_result # Printing data
unique(student_result)

~~~
{: .language-r}

~~~
    name maths science history
1    Ram     7       5       7
2  Geeta     8       7       7
3   John     8       6       7
4   Paul     9       8       7
5 Cassie    10       9       7
6  Geeta     8       7       7
7   Paul     9       8       7

    name maths science history
1    Ram     7       5       7
2  Geeta     8       7       7
3   John     8       6       7
4   Paul     9       8       7
5 Cassie    10       9       7
~~~
{: .output}

## How to Combine Two Columns into One

Using paste() function is used to join the two columns in the dataframe with a separator.

~~~
data = data.frame(firstname=c("akash", "kyathi", "preethi"),
lastname=c("deep", "lakshmi", "savithri"),
marks=c(89, 96, 89))
print(data)# display

data$fullname = paste(data$firstname, data$lastname, sep=" ")# combine first name and last name columns

data # display
~~~
{: .language-r}

~~~
   firstname lastname marks
1     akash     deep    89
2    kyathi  lakshmi    96
3   preethi savithri    89

  firstname lastname marks         fullname
1     akash     deep    89       akash deep
2    kyathi  lakshmi    96   kyathi lakshmi
3   preethi savithri    89 preethi savithri
~~~
{: .output}

## How to convert a column to binaries 
To do this we us the ifelse method
~~~
data = data.frame(firstname=c("akash", "kyathi", "preethi"),
lastname=c("deep", "lakshmi", "savithri"),
marks=c(55, 45, 80),
subject=c("maths","science", "maths"))

data$markspass <- ifelse(data$marks <= 50, 0, 1)
print(data)# display
data$subject_maths <- ifelse(data$subject == "maths", 0, 1)

data # display
~~~
{: .language-r}

~~~
  firstname lastname marks subject markspass
1     akash     deep    55   maths         1
2    kyathi  lakshmi    45 science         0
3   preethi savithri    80   maths         1

  firstname lastname marks subject markspass subject_maths
1     akash     deep    55   maths         1             0
2    kyathi  lakshmi    45 science         0             1
3   preethi savithri    80   maths         1             0
~~~
{: .output}

## Assessing machine learning models with AIC

### What is AIC?

AIC is most commonly used when evaluating a model’s performance on a test set is difficult, such as in small datasets or time series analysis. It is especially useful for time series because the most valuable data points are often the most recent, which are typically reserved for validation and testing. By training on the entire dataset and using AIC, model selection can be improved compared to traditional train/validation/test approaches.

AIC assesses a model’s fit on the training data while incorporating a penalty for complexity, similar to regularisation. The goal is to minimise AIC, striking the best balance between model fit and generalisability. This ultimately helps maximise performance on out-of-sample data.

<img src="https://latex.codecogs.com/svg.image?AIC=-2ln(L)&plus;2k" />

AIC evaluates a model’s fit using its maximum likelihood estimation (log-likelihood). Log-likelihood quantifies how probable the observed data is given the model, with higher values indicating a better fit. The natural logarithm of the likelihood is used for computational simplicity.

Models with higher log-likelihoods tend to have lower AIC values, meaning they fit the data well. However, AIC also includes a penalty for model complexity—models with more parameters are more prone to over-fitting. This balance helps identify models that generalise better to unseen data.

### when should you use AIC?

AIC is commonly used when out-of-sample data is unavailable, when comparing multiple model types, or for efficiency. Recently, I used AIC to quickly evaluate several seasonal autoregressive integrated moving average (SARIMA) models to determine the best baseline while keeping the full dataset in my training set.

When applying AIC to SARIMA models, it's important to recognize that AIC assumes all models are trained on the same data. This means using AIC to compare models with different orders of differencing is technically invalid, as each additional differencing order removes a data point. To use AIC correctly, you must ensure its assumptions are met. AIC assumes that you:

* Are using the same data between models.
* Are measuring the same outcome variable between models.
* Have a sample of infinite size.

The last assumption exists because AIC converges to the correct solution as the sample size approaches infinity. In practice, a large enough sample can provide a good approximation. However, since AIC is often used in small-sample scenarios, an adjusted version called AICc is available. AICc includes a correction term that aligns with AIC for large samples but provides a more accurate estimate for smaller ones.

As a general rule, it’s safest to use AICc by default. It becomes especially important when the ratio of data points (n) to the number of parameters (k) is less than 40.

Once the assumptions of AIC (or AICc) are met, one of its biggest advantages is that models do not need to be nested for valid comparisons. This contrasts with other single-number measures of model fit, such as the likelihood-ratio test, which requires nested models (where one model's parameters are a subset of another's). Because of this, AIC allows for direct comparison between vastly different models.

### How Should AIC Results Be Interpreted?

Once you have a set of AIC scores, what’s the next step? Should you simply choose the model with the lowest score? While that’s an option, AIC scores provide a probabilistic ranking of models based on their likelihood of minimizing information loss (i.e., best fitting the data). This concept is better understood through the formula below.

Suppose you have calculated AIC scores for multiple models, resulting in a series (AIC₁, AIC₂, …, AICₙ). For any given AICᵢ, you can determine the probability that the “i-th” model minimizes information loss using the following formula, where AICₘᵢₙ represents the lowest AIC score in the set.

<img src="https://latex.codecogs.com/svg.image?P=e^{\frac{(AIC_{min}-AIC_i)}{2}}" />

For example, suppose you have three candidate models with AIC values of 100, 102, and 110. The second model is exp((100 − 102)/2) = 0.368 times as likely as the first model to minimize information loss. Similarly, the third model is exp((100 − 110)/2) = 0.007 times as likely as the first model to do so.

This illustrates that AIC alone cannot definitively determine whether one model is better than another—it relies solely on in-sample data. However, there are strategies to interpret and handle these probabilistic results effectively:

* Set an alpha level that, below which, competing models will be dismissed. Alpha = 0.05, for instance, would dismiss the 110-score model at 0.007.
* If you find competing models above your alpha level, you can create a weighted sum of your models in proportion to their probability. A 1:0.368, in the case of the 100 and 102-scored models.

If absolute precision isn’t critical and you simply want to choose the model with the lowest AIC, be aware that a small difference in AIC scores suggests a higher likelihood of selecting a suboptimal model. When multiple models have AIC scores close to the minimum, the distinction between them is less clear. For instance, a score of 100 versus 100.1 may not strongly favour one model over the other, whereas a comparison between 100 and 120 would indicate a much clearer preference.

### Problems with AIC

Remember, AIC only measures the relative quality of models, meaning that even the best model in your comparison could still have a poor fit. To ensure your model meets an acceptable absolute standard, additional metrics, such as Mean Absolute Percentage Error (MAPE), should be used.

AIC is also a relatively simple calculation and has been expanded upon by more advanced, computationally intensive methods that often provide greater accuracy. Examples include the Deviance Information Criterion (DIC), Watanabe-Akaike Information Criterion (WAIC), and Leave-One-Out Cross-Validation (LOO-CV), which AIC asymptotically approaches as sample size increases.

The choice between AIC and these newer methods depends on your priorities—whether you prioritise accuracy, computational efficiency, or the ease of calculation based on your software’s capabilities. In most cases where sufficient data is available, the best way to evaluate model performance is through traditional machine learning practices, using train, validation, and test sets. However, when such an approach isn’t feasible—such as in small datasets or time series analysis—AIC can be a valuable alternative for model evaluation.

## Variable selection functions

### Drop1 method

The given AIC from drop1 relates to the whole model - not to a variable, so the output tells you which variable to remove in order to yield the model with the lowest AIC. For example, with the built-in dataset swiss

~~~
lm1 <- lm(Fertility ~ ., data = swiss)
drop1(lm1, test = "F")  # So called 'type II' anova
~~~
{: .language-r}

~~~
Single term deletions

Model:
Fertility ~ Agriculture + Examination + Education + Catholic + 
    Infant.Mortality
                 Df Sum of Sq    RSS    AIC F value    Pr(>F)    
<none>                        2105.0 190.69                      
Agriculture       1    307.72 2412.8 195.10  5.9934  0.018727 *  
Examination       1     53.03 2158.1 189.86  1.0328  0.315462    
Education         1   1162.56 3267.6 209.36 22.6432 2.431e-05 ***
Catholic          1    447.71 2552.8 197.75  8.7200  0.005190 ** 
Infant.Mortality  1    408.75 2513.8 197.03  7.9612  0.007336 ** 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
~~~
{: .output}

{% include links.md %}
