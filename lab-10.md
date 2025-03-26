Lab 10 - Grading the professor, Pt. 2
================
Fiona Wang
March 24th, 2025

## Load packages and data

``` r
library(tidyverse) 
library(tidymodels)
library(openintro)
```

## Exercise 1

Load data

``` r
data <- evals
```

Simple linear regression m_bty:

``` r
m_bty <- linear_reg() %>% 
  set_engine("lm") %>% 
  fit(score ~ bty_avg, data = data)
tidy(m_bty)
```

    ## # A tibble: 2 × 5
    ##   term        estimate std.error statistic   p.value
    ##   <chr>          <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)   3.88      0.0761     51.0  1.56e-191
    ## 2 bty_avg       0.0666    0.0163      4.09 5.08e-  5

``` r
glance(m_bty)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.squared sigma statistic   p.value    df logLik   AIC   BIC
    ##       <dbl>         <dbl> <dbl>     <dbl>     <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1    0.0350        0.0329 0.535      16.7 0.0000508     1  -366.  738.  751.
    ## # ℹ 3 more variables: deviance <dbl>, df.residual <int>, nobs <int>

The linear model is: score = 0.067(bty_avg) + 3.88.  
R^2 is 0.035 whereas the adjusted R^2 is 0.033.

## Exercise 2

Multiple regression model

``` r
m_bty_gen <- linear_reg() %>% 
  set_engine("lm") %>% 
  fit(score ~ bty_avg * gender, data = data)
tidy(m_bty_gen)
```

    ## # A tibble: 4 × 5
    ##   term               estimate std.error statistic   p.value
    ##   <chr>                 <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)          3.95      0.118      33.5  2.92e-125
    ## 2 bty_avg              0.0306    0.0240      1.28 2.02e-  1
    ## 3 gendermale          -0.184     0.153      -1.20 2.32e-  1
    ## 4 bty_avg:gendermale   0.0796    0.0325      2.45 1.46e-  2

``` r
glance(m_bty_gen)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.squared sigma statistic     p.value    df logLik   AIC   BIC
    ##       <dbl>         <dbl> <dbl>     <dbl>       <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1    0.0713        0.0652 0.526      11.7 0.000000200     3  -357.  725.  745.
    ## # ℹ 3 more variables: deviance <dbl>, df.residual <int>, nobs <int>

This multiple regression: score = 0.0306(bty_avg) - 0.1835(gender) +
0.080(bty_avg)(gender) + 3.95 The R^2 is 0.071 whereas the adjusted R^2
is 0.065.

### Exercise 3 (fix from here)

Based on the result from the last exercise, there are three slopes.  
The first slope is 0.0306 for average beauty rating. This means that
with one unit increase in the average beauty rating, there is a 0.0306
unit increase in the score while all else is held constant.  
The second slope is -0.1835 for gender. This means that with one unit
increase in gender, there is a 0.1835 unit decrease in the score while
all else is held constant. As we know from the last lab, female is the
reference group, whereas male is 1 unit higher than female. So, male
will be 0.1835 lower on average in score than females while all else is
held constant.  
The slope for the interaction term is 0.080. This means that there is an
interaction effect between average beauty rating and gender. With one
unit increase in gender, the slope for average beauty rating will
increase 0.08. So, for males, the slope for beauty average rating is
0.1106, while for females, the slope is still 0.0306. The intercept is
3.95. This means that when the average beauty rating is 0, and that the
professor is a female, this professor will have a rating of 3.95.

### Exercise 4

According to the R^2 values, 7.1% of the variance in score is explained
by the model m_bty_gen, but this is a more optimistic interpretation. If
we look at the adjusted R^2, this model explains 6.5% of the variance in
score.

### Exercise 5

For male professors, the line of best fit is:  
score = 0.0306(bty_avg) - 0.1835(gender) + 0.080(bty_avg)(gender) +
3.95  
Since males are gender = 1, we can further write the equation as:  
score = 0.1106(bty_avg) + 3.7665

### Exercise 6

Since I added the interaction term, there is no single answer for this
question. For two professors who received 5 for their average beauty
rating, males tended to have a higher course evaluation score. For two
professors who received 1 for their average beauty rating, females
tended to have a higher course evaluation score.

### Exercise 7

According to our equation, we know that the relationship between beauty
average rating and evaluation score is a positive relationship, slope =
0.0306, while all else held constant. However, as indicated by the
interaction term, the relationship between beauty average rating and
evaluation score increases 0.08. For males, this relationship is
stronger than for females.
