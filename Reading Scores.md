Reading Scores Prediction
================
2022-12-15

# Overview

The Programme for International Student Assessment (PISA) is a test that
is administered every three years to 15-year-old students from around
the world. The test evaluates students’ performance in mathematics,
reading, and science and provides a quantitative way to compare the
performance of students from different regions. In this homework
assignment, we will use the pisa2009train.csv and pisa2009test.csv
datasets to predict the reading scores of American students who took the
PISA exam in 2009. These datasets contain information about the
demographics and schools of the students, but are not supposed to
include any identifying information. By using these datasets, you are
agreeing to the NCES data use agreement, which prohibits attempting to
determine the identity of any student in the datasets.

# About the Data Set

The pisa2009train.csv and pisa2009test.csv datasets contain information
about students who took the PISA exam in 2009. Each row in the dataset
represents one student, and the datasets include a variety of variables
that provide information about the student’s background, school, and
performance on the exam. Some of the variables in the datasets include:

grade: The grade in school of the student (most 15-year-olds in America
are in 10th grade) male: Whether the student is male (1/0) raceeth: The
race/ethnicity composite of the student preschool: Whether the student
attended preschool (1/0) expectBachelors: Whether the student expects to
obtain a bachelor’s degree (1/0) motherHS: Whether the student’s mother
completed high school (1/0) motherBachelors: Whether the student’s
mother obtained a bachelor’s degree (1/0) motherWork: Whether the
student’s mother has part-time or full-time work (1/0) fatherHS: Whether
the student’s father completed high school (1/0) fatherBachelors:
Whether the student’s father obtained a bachelor’s degree (1/0)
fatherWork: Whether the student’s father has part-time or full-time work
(1/0) selfBornUS: Whether the student was born in the United States of
America (1/0) motherBornUS: Whether the student’s mother was born in the
United States of America (1/0) fatherBornUS: Whether the student’s
father was born in the United States of America (1/0) englishAtHome:
Whether the student speaks English at home (1/0) computerForSchoolwork:
Whether the student has access to a computer for schoolwork (1/0)
read30MinsADay: Whether the student reads for pleasure for 30
minutes/day (1/0) minutesPerWeekEnglish: The number of minutes per week
the student spend in English class studentsInEnglish: The number of
students in this student’s English class at school schoolHasLibrary:
Whether this student’s school has a library (1/0) publicSchool: Whether
this student attends a public school (1/0) urban: Whether this student’s
school is in an urban area (1/0) schoolSize: The number of students in
this student’s school readingScore: The student’s reading score, on a
1000-point scale

# EDA

``` r
# Loading the Data Sets 
pisaTrain = read.csv("pisa2009train.csv")
pisaTest = read.csv("pisa2009test.csv")

# Exploring the number of Students in the training set
nrow(pisaTrain)
```

    ## [1] 3663

As we can see the number of the students in the training set is 3663.  
Now lets explore the data a bit

``` r
library(knitr)
#the avreage reading test score of males and females in the Training set
z = tapply(pisaTrain$readingScore, pisaTrain$male, mean)
kable(z)
```

|     |        x |
|:----|---------:|
| 0   | 512.9406 |
| 1   | 483.5325 |

As we can see the avreage reading score of males is 483.5325 and the
avreage reading score of females is 512.9406.  
Now lets look if there’s any missing value in our data set.

``` r
kable(summary(pisaTrain))
```

|     | grade         | male           | raceeth          | preschool      | expectBachelors | motherHS     | motherBachelors | motherWork     | fatherHS       | fatherBachelors | fatherWork     | selfBornUS     | motherBornUS   | fatherBornUS   | englishAtHome  | computerForSchoolwork | read30MinsADay | minutesPerWeekEnglish | studentsInEnglish | schoolHasLibrary | publicSchool   | urban          | schoolSize   | readingScore  |
|:----|:--------------|:---------------|:-----------------|:---------------|:----------------|:-------------|:----------------|:---------------|:---------------|:----------------|:---------------|:---------------|:---------------|:---------------|:---------------|:----------------------|:---------------|:----------------------|:------------------|:-----------------|:---------------|:---------------|:-------------|:--------------|
|     | Min. : 8.00   | Min. :0.0000   | Length:3663      | Min. :0.0000   | Min. :0.0000    | Min. :0.00   | Min. :0.0000    | Min. :0.0000   | Min. :0.0000   | Min. :0.0000    | Min. :0.0000   | Min. :0.0000   | Min. :0.0000   | Min. :0.0000   | Min. :0.0000   | Min. :0.0000          | Min. :0.0000   | Min. : 0.0            | Min. : 1.0        | Min. :0.0000     | Min. :0.0000   | Min. :0.0000   | Min. : 100   | Min. :168.6   |
|     | 1st Qu.:10.00 | 1st Qu.:0.0000 | Class :character | 1st Qu.:0.0000 | 1st Qu.:1.0000  | 1st Qu.:1.00 | 1st Qu.:0.0000  | 1st Qu.:0.0000 | 1st Qu.:1.0000 | 1st Qu.:0.0000  | 1st Qu.:1.0000 | 1st Qu.:1.0000 | 1st Qu.:1.0000 | 1st Qu.:1.0000 | 1st Qu.:1.0000 | 1st Qu.:1.0000        | 1st Qu.:0.0000 | 1st Qu.: 225.0        | 1st Qu.:20.0      | 1st Qu.:1.0000   | 1st Qu.:1.0000 | 1st Qu.:0.0000 | 1st Qu.: 712 | 1st Qu.:431.7 |
|     | Median :10.00 | Median :1.0000 | Mode :character  | Median :1.0000 | Median :1.0000  | Median :1.00 | Median :0.0000  | Median :1.0000 | Median :1.0000 | Median :0.0000  | Median :1.0000 | Median :1.0000 | Median :1.0000 | Median :1.0000 | Median :1.0000 | Median :1.0000        | Median :0.0000 | Median : 250.0        | Median :25.0      | Median :1.0000   | Median :1.0000 | Median :0.0000 | Median :1212 | Median :499.7 |
|     | Mean :10.09   | Mean :0.5111   | NA               | Mean :0.7228   | Mean :0.7859    | Mean :0.88   | Mean :0.3481    | Mean :0.7345   | Mean :0.8593   | Mean :0.3319    | Mean :0.8531   | Mean :0.9313   | Mean :0.7725   | Mean :0.7668   | Mean :0.8717   | Mean :0.8994          | Mean :0.2899   | Mean : 266.2          | Mean :24.5        | Mean :0.9676     | Mean :0.9339   | Mean :0.3849   | Mean :1369   | Mean :497.9   |
|     | 3rd Qu.:10.00 | 3rd Qu.:1.0000 | NA               | 3rd Qu.:1.0000 | 3rd Qu.:1.0000  | 3rd Qu.:1.00 | 3rd Qu.:1.0000  | 3rd Qu.:1.0000 | 3rd Qu.:1.0000 | 3rd Qu.:1.0000  | 3rd Qu.:1.0000 | 3rd Qu.:1.0000 | 3rd Qu.:1.0000 | 3rd Qu.:1.0000 | 3rd Qu.:1.0000 | 3rd Qu.:1.0000        | 3rd Qu.:1.0000 | 3rd Qu.: 300.0        | 3rd Qu.:30.0      | 3rd Qu.:1.0000   | 3rd Qu.:1.0000 | 3rd Qu.:1.0000 | 3rd Qu.:1900 | 3rd Qu.:566.2 |
|     | Max. :12.00   | Max. :1.0000   | NA               | Max. :1.0000   | Max. :1.0000    | Max. :1.00   | Max. :1.0000    | Max. :1.0000   | Max. :1.0000   | Max. :1.0000    | Max. :1.0000   | Max. :1.0000   | Max. :1.0000   | Max. :1.0000   | Max. :1.0000   | Max. :1.0000          | Max. :1.0000   | Max. :2400.0          | Max. :75.0        | Max. :1.0000     | Max. :1.0000   | Max. :1.0000   | Max. :6694   | Max. :746.0   |
|     | NA            | NA             | NA               | NA’s :56       | NA’s :62        | NA’s :97     | NA’s :397       | NA’s :93       | NA’s :245      | NA’s :569       | NA’s :233      | NA’s :69       | NA’s :71       | NA’s :113      | NA’s :71       | NA’s :65              | NA’s :34       | NA’s :186             | NA’s :249         | NA’s :143        | NA             | NA             | NA’s :162    | NA            |

From the table above we can see which variables have how many missing
values (NA).  
Now, let’s erase the records with missing values

``` r
pisaTrain = na.omit(pisaTrain)
pisaTest = na.omit(pisaTest)
nrow(pisaTrain)
```

    ## [1] 2414

As we can see the records had dropped from 3663 to 2414. Meaning we have
more than 30% non-complete record which can be explained by the fact
that these surveys are not completly filled most of the time by the
survey takers.

### Factor Variables

Factor variables are variables that can take on a specific set of
values. For example, a “Region” variable is a factor variable because it
can only take on a certain number of values, such as “North America” or
“Africa”. These values are called levels.

A factor variable can either be unordered or ordered. An unordered
factor, like the “Region” variable, does not have a natural ordering
between its levels. This means that the levels can be arranged in any
order and the variable will still have the same meaning. An ordered
factor, on the other hand, has a natural ordering between its levels.
For example, the classifications “large,” “medium,” and “small” are
ordered because there is a clear hierarchy between the levels (large is
greater than medium, which is greater than small). This means that the
levels must be arranged in a specific order for the variable to have
meaning.

In a linear regression model, unordered factor variables need to be
encoded in a specific way to be used as predictors. One approach is to
define one level of the factor as the reference level and add a binary
variable for each of the remaining levels. This means that a factor with
n levels is replaced by n-1 binary variables. The reference level is
typically chosen to be the most frequently occurring level in the
dataset.

For example, suppose we have an unordered factor variable called “color”
with levels “red”, “green”, and “blue”. If “green” is selected as the
reference level, we would add binary variables “colorred” and
“colorblue” to the linear regression model. For each red example,
“colorred” would be set to 1 and “colorblue” would be set to 0. For each
blue example, “colorred” would be set to 0 and “colorblue” would be set
to 1. For each green example, both “colorred” and “colorblue” would be
set to 0.

In the data set, the variable “raceeth” has levels “American
Indian/Alaska Native”, “Asian”, “Black”, “Hispanic”, “More than one
race”, “Native Hawaiian/Other Pacific Islander”, and “White”. Because
“White” is the most common level in our population, we would select it
as the reference level and encode the remaining levels as binary
variables. This would allow us to include “raceeth” as a predictor in
our linear regression model.

Basically, we are creating a binary variable for each level except for
the refernce level. In this case, for the variable raceeth, all but
White. Meaning, a student who is Asian will have a value of 1 for the
raceethAsian variable, and all other raceeth binary variables will have
a value of 0. In contrast, a white student will have a value of 0 for
all raceeth binary variables, since “White” is the reference level in
this context.

# Building a Model

``` r
#Converting the raceeth variable into factor in the data sets
pisaTrain$raceeth = as.factor(pisaTrain$raceeth)
pisaTest$raceeth = as.factor(pisaTest$raceeth)

#Ordering the factors with reference to "White"
pisaTrain$raceeth = relevel(pisaTrain$raceeth, "White")
pisaTest$raceeth = relevel(pisaTest$raceeth, "White")

# Training a Linear Regression Model
lmodel = lm(readingScore ~. , data = pisaTrain)
summary(lmodel)
```

    ## 
    ## Call:
    ## lm(formula = readingScore ~ ., data = pisaTrain)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -247.44  -48.86    1.86   49.77  217.18 
    ## 
    ## Coefficients:
    ##                                                 Estimate Std. Error
    ## (Intercept)                                   143.766333  33.841226
    ## grade                                          29.542707   2.937399
    ## male                                          -14.521653   3.155926
    ## raceethAmerican Indian/Alaska Native          -67.277327  16.786935
    ## raceethAsian                                   -4.110325   9.220071
    ## raceethBlack                                  -67.012347   5.460883
    ## raceethHispanic                               -38.975486   5.177743
    ## raceethMore than one race                     -16.922522   8.496268
    ## raceethNative Hawaiian/Other Pacific Islander  -5.101601  17.005696
    ## preschool                                      -4.463670   3.486055
    ## expectBachelors                                55.267080   4.293893
    ## motherHS                                        6.058774   6.091423
    ## motherBachelors                                12.638068   3.861457
    ## motherWork                                     -2.809101   3.521827
    ## fatherHS                                        4.018214   5.579269
    ## fatherBachelors                                16.929755   3.995253
    ## fatherWork                                      5.842798   4.395978
    ## selfBornUS                                     -3.806278   7.323718
    ## motherBornUS                                   -8.798153   6.587621
    ## fatherBornUS                                    4.306994   6.263875
    ## englishAtHome                                   8.035685   6.859492
    ## computerForSchoolwork                          22.500232   5.702562
    ## read30MinsADay                                 34.871924   3.408447
    ## minutesPerWeekEnglish                           0.012788   0.010712
    ## studentsInEnglish                              -0.286631   0.227819
    ## schoolHasLibrary                               12.215085   9.264884
    ## publicSchool                                  -16.857475   6.725614
    ## urban                                          -0.110132   3.962724
    ## schoolSize                                      0.006540   0.002197
    ##                                               t value Pr(>|t|)    
    ## (Intercept)                                     4.248 2.24e-05 ***
    ## grade                                          10.057  < 2e-16 ***
    ## male                                           -4.601 4.42e-06 ***
    ## raceethAmerican Indian/Alaska Native           -4.008 6.32e-05 ***
    ## raceethAsian                                   -0.446  0.65578    
    ## raceethBlack                                  -12.271  < 2e-16 ***
    ## raceethHispanic                                -7.528 7.29e-14 ***
    ## raceethMore than one race                      -1.992  0.04651 *  
    ## raceethNative Hawaiian/Other Pacific Islander  -0.300  0.76421    
    ## preschool                                      -1.280  0.20052    
    ## expectBachelors                                12.871  < 2e-16 ***
    ## motherHS                                        0.995  0.32001    
    ## motherBachelors                                 3.273  0.00108 ** 
    ## motherWork                                     -0.798  0.42517    
    ## fatherHS                                        0.720  0.47147    
    ## fatherBachelors                                 4.237 2.35e-05 ***
    ## fatherWork                                      1.329  0.18393    
    ## selfBornUS                                     -0.520  0.60331    
    ## motherBornUS                                   -1.336  0.18182    
    ## fatherBornUS                                    0.688  0.49178    
    ## englishAtHome                                   1.171  0.24153    
    ## computerForSchoolwork                           3.946 8.19e-05 ***
    ## read30MinsADay                                 10.231  < 2e-16 ***
    ## minutesPerWeekEnglish                           1.194  0.23264    
    ## studentsInEnglish                              -1.258  0.20846    
    ## schoolHasLibrary                                1.318  0.18749    
    ## publicSchool                                   -2.506  0.01226 *  
    ## urban                                          -0.028  0.97783    
    ## schoolSize                                      2.977  0.00294 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 73.81 on 2385 degrees of freedom
    ## Multiple R-squared:  0.3251, Adjusted R-squared:  0.3172 
    ## F-statistic: 41.04 on 28 and 2385 DF,  p-value: < 2.2e-16

### Calculating RMSE

``` r
SSE = sum(lmodel$residuals^2)
RMSE = sqrt(SSE/nrow(pisaTrain))
RMSE
```

    ## [1] 73.36555

The RMSE is 73.37

### Understanding the Model

- In the model, two students that are on different grades but have all
  the other variables as identical will have a different reading score
  by $(number of grades)*(Factor of grades variable)$ Which in this case
  happen to be 29.54.
- The Model is to expect that if two students have all the variable
  identical except for the race of the first being Asian and the second
  is White. The former will get lower score by a factor of -4.11.

# Applying the model on the Test set

``` r
predTest = predict(lmodel, newdata = pisaTest)
```

### Let’s see how good it is

``` r
SSE = sum((predTest - pisaTest$readingScore)^2)
RMSE1 = sqrt(mean((predTest-pisaTest$readingScore)^2))
RMSE1
```

    ## [1] 76.29079

``` r
#BaseLine model RMSE
baseline = mean(pisaTest$readingScore)
RMSE2 = sqrt(mean((baseline-pisaTest$readingScore)^2))
RMSE2
```

    ## [1] 88.75557

While not much sophisticated, this simple linear model has lower RMSE
than a base model.
