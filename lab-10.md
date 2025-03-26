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

### Part 2

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

## Exercise 3 (fix from here)

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

## Exercise 4

According to the R^2 values, 7.1% of the variance in score is explained
by the model m_bty_gen, but this is a more optimistic interpretation. If
we look at the adjusted R^2, this model explains 6.5% of the variance in
score.

## Exercise 5

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

## Exercise 7

According to our equation, we know that the relationship between beauty
average rating and evaluation score is a positive relationship, slope =
0.0306, while all else held constant. However, as indicated by the
interaction term, the relationship between beauty average rating and
evaluation score increases 0.08. For males, this relationship is
stronger than for females. \## Exercise 8 The adjusted R^2 for m_bty is
0.033, and the adjusted R^2 for m_bty_gen is 0.065. There is an increase
in R^2, meaning that gender and the interaction between gender and
beauty average scoring explain the evaluation score over and above
beauty average score.

## Exercise 9

In the first model, the parameter estimate is 0.067, whereas in the
second model, the parameter estimate is 0.0306. Yes, adding gender and
the interaction term changed the slope.

## Exercise 10

``` r
m_bty_rank <- linear_reg() %>% 
  set_engine("lm") %>% 
  fit(score ~ bty_avg + rank, data = data)
tidy(m_bty_rank)
```

    ## # A tibble: 4 × 5
    ##   term             estimate std.error statistic   p.value
    ##   <chr>               <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)        3.98      0.0908     43.9  2.92e-166
    ## 2 bty_avg            0.0678    0.0165      4.10 4.92e-  5
    ## 3 ranktenure track  -0.161     0.0740     -2.17 3.03e-  2
    ## 4 ranktenured       -0.126     0.0627     -2.01 4.45e-  2

``` r
glance(m_bty_rank)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.squared sigma statistic   p.value    df logLik   AIC   BIC
    ##       <dbl>         <dbl> <dbl>     <dbl>     <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1    0.0465        0.0403 0.533      7.46 0.0000688     3  -363.  737.  758.
    ## # ℹ 3 more variables: deviance <dbl>, df.residual <int>, nobs <int>

The equation for : score = 0.068(beauty average scoring) - 0.161(tenure
track) - 0.126(tenured) + 3.982.  
There are three slopes.  
The first one is 0.068, this means that with one unit increase in the
beauty average scoring, there is a 0.068 unit increase in the course
evals score, while all else held constant.  
The second slope is -0.161, this means that when someone’s rank is
tenure track, they will have a 0.161 decrease, on average, in their eval
score, while all else held constant.  
The third slope is -0.126, this means that when someone’s rank is
tenured, they will have a 0.126 decrease, on average, in their eval
score, while all else held constant.  
The intercept is 3.982. This means that when someone has the rank of
teaching, and have a beauty average score of 0, they will have a 3.982
rating for course evals on average.

### Part 3

## Exercise 11

While looking at the descriptions for all these variables. I think many
of them are not directly linked to course evals, but could indirectly
influence students’ perceptions of the course and professor.  
Among these, I think ethnicity is the worst predictor. I don’t think
there will be any association between ethnicity and course eval score.

## Exercise 12

``` r
m_eth <- linear_reg() %>% 
  set_engine("lm") %>% 
  fit(score ~ ethnicity, data = data)
tidy(m_eth)
```

    ## # A tibble: 2 × 5
    ##   term                  estimate std.error statistic   p.value
    ##   <chr>                    <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)              4.07     0.0679     60.0  6.02e-220
    ## 2 ethnicitynot minority    0.119    0.0731      1.63 1.03e-  1

``` r
glance(m_eth)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.squared sigma statistic p.value    df logLik   AIC   BIC
    ##       <dbl>         <dbl> <dbl>     <dbl>   <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1   0.00575       0.00359 0.543      2.67   0.103     1  -373.  752.  765.
    ## # ℹ 3 more variables: deviance <dbl>, df.residual <int>, nobs <int>

According to the output, this model is not significant, and that
ethnicity is not a significant predictor of score.

## Exercise 13

I will not include cls_did_eval as an additional predictor. This is
because if we have cls_students and cls_perc_eval, we can infer the
number of students in class who completed evaluation. Any variance in
score that cls_did_eval is able to explain would have already been
explained by cls_perc_eval and cls_students. Adding it will not lead to
a inrease in R^2, and itself won’t arise as a unique predictor.  
I also want to exclude ethnicity as a predictor because I already know
it is not a significant predictor.

## Exercise 14
