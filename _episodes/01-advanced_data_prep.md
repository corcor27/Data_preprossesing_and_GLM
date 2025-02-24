---
title: "Advanced data preparation"
teaching: 20
exercises: 0
questions:
- What is machine learning?
objectives:
- "s"

keypoints:
- "a"

---

## Dataframes

Data Frames are data displayed in a format as a table and are a powerful tool when it comes to performing more advanced computational methods. 

### Creating a Dataframe

Data Frames can have different types of data inside it. While the first column can be character, the second and third can be numeric or logical. However, each column should have the same type of data.

~~~
> Data_Frame <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45))

# Print the data frame
> Data_Frame 
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
> Data_Frame <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45))

> Data_Frame

> summary(Data_Frame) 
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
> Data_Frame <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45))

> Data_Frame[1]

> Data_Frame[["Training"]]

> Data_Frame$Training 
~~~
{: .language-r}

~~~
> Data_Frame[1]
  Training
1 Strength
2  Stamina
3    Other
> 
> Data_Frame[["Training"]]
[1] "Strength" "Stamina"  "Other"   
> 
> Data_Frame$Training 
[1] "Strength" "Stamina"  "Other"   
~~~
{: .output}

### Adding rows
Use the rbind() function to add new rows in a Data Frame:
~~~
> Data_Frame <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45))

# Add a new row
> New_row_DF <- rbind(Data_Frame, c("Strength", 110, 110))

# Print the new row
> New_row_DF 
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
> Data_Frame <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45)) 
> New_col_DF <- cbind(Data_Frame, Steps = c(1000, 6000, 2000))# Add a new column
> New_col_DF # Print the new column
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
> Data_Frame <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45))

> Data_Frame_New <- Data_Frame[-c(1), -c(1)]# Remove the first row and column

> Data_Frame_New # Print the new data frame
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
> Data_Frame <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45))

> dim(Data_Frame)
> ncol(Data_Frame)
> nrow(Data_Frame)
> length(Data_Frame)
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
> Data_Frame1 <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45))

> Data_Frame2 <- data.frame (
> Training = c("Stamina", "Stamina", "Strength"),
> Pulse = c(140, 150, 160),
> Duration = c(30, 30, 20))

> New_Data_Frame <- rbind(Data_Frame1, Data_Frame2)
> New_Data_Frame 
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
> Data_Frame3 <- data.frame (
> Training = c("Strength", "Stamina", "Other"),
> Pulse = c(100, 150, 120),
> Duration = c(60, 30, 45))

> Data_Frame4 <- data.frame (
> Steps = c(3000, 6000, 2000),
> Calories = c(300, 400, 300))

> New_Data_Frame1 <- cbind(Data_Frame3, Data_Frame4)
> New_Data_Frame1 
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
> df1= data.frame(  
> A1 = c(NA, 10, NA, 7, 8, 11,20),
> A2 = c("A", 9, 3, "B", "C", "D","E"),
> A3 = c(1, 0, NA, 1, 1, NA,3))
> print(df1) #printing the dataframe

> print("After removing the NA values ")
> result=na.omit(df1)
> print(result)
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
> df1 <- data.frame(  
> A1 = c(NA, 10, NA, 7, 8, 11,20),
> A2 = c("A", 9, 3, "B", "C", "D","E"),
> A3 = c(1, 0, NA, 1, 1, NA,3))
> print(df1)#printing the dataframe

> print("After removing the NA values ")
> result=df1[complete.cases(df1),]
> print(result)
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

> vector_data <- c(1, 2, 3, 4, 4, 5) # Create a sample vector with duplicate elements
> duplicated(vector_data) # Identify duplicate elements
> sum(duplicated(vector_data)) # count of duplicated data

~~~
{: .language-r}

~~~
[1] FALSE FALSE FALSE FALSE  TRUE FALSE
[1] 1
~~~
{: .output}

### Removing Duplicate Data in vector

~~~

> vector_data <- c(1, 2, 3, 4, 4, 5)
> unique(vector_data)# Remove duplicate elements

~~~
{: .language-r}

~~~
[1] 1 2 3 4 5
~~~
{: .output}

### Identifying Duplicate Data in a data frame

~~~

> student_result=data.frame(name=c("Ram","Geeta","John","Paul",
>                                  "Cassie","Geeta","Paul"),
>                           maths=c(7,8,8,9,10,8,9),
>                           science=c(5,7,6,8,9,7,8),
>                           history=c(7,7,7,7,7,7,7))
 

> student_result # Printing data
> duplicated(student_result)
> sum(duplicated(student_result))

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
> student_result=data.frame(name=c("Ram","Geeta","John","Paul",
>                                  "Cassie","Geeta","Paul"),
>                           maths=c(7,8,8,9,10,8,9),
>                           science=c(5,7,6,8,9,7,8),
>                           history=c(7,7,7,7,7,7,7))
 

> student_result # Printing data
> unique(student_result)

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

{% include links.md %}
