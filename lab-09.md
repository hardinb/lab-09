Lab 9
================
Ben Hardin
2023-03-10

``` r
library(tidyverse) 
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.4.0     ✔ purrr   0.3.5
    ## ✔ tibble  3.1.8     ✔ dplyr   1.1.0
    ## ✔ tidyr   1.2.1     ✔ stringr 1.5.0
    ## ✔ readr   2.1.3     ✔ forcats 0.5.2
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(broom)
library(openintro)
```

    ## Loading required package: airports
    ## Loading required package: cherryblossom
    ## Loading required package: usdata

### Exercise 1

Examining the distribution of course evaluation scores, we can see that
there is a right skew, with the vast majority of people giving
relatively high course evaluations. In other words, students generally
almost never rate courses below the midpoint of the scale, and often
rate courses at least 4 out of 5 or higher. I’m not too surprised by
this, because I assume students would usually want to be some
combination of kind and accurate in their evaluations (at least, in
their quantitative ratings), and also because I assume most college
instructors are usually quite competent at their job.

``` r
ggplot(evals, aes(x = score))+
  geom_histogram()+
  scale_x_continuous(limits = c(1, 5))
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 2 rows containing missing values (`geom_bar()`).

![](lab-09_files/figure-gfm/score-dist-1.png)<!-- -->

### Exercise 2

The scatterplot below shows the relationship between course evals and
average beauty ratings for each instructor.

``` r
ggplot(evals, aes(x = score, y = bty_avg))+
  geom_point()
```

![](lab-09_files/figure-gfm/score-bty-1.png)<!-- -->

### Exercise 3

The same scatterplot is shown below, this time with the points
“jittered”. Jittering means that some slight random noise had been added
to the data, which means that datapoints with the same value no longer
overlap eachother in the graph. This allows us to see where there is a
greater density of datapoints, in this case showing that there are quite
a few more datapoints for high course evaluation scores and relatively
few datapoints for more middling evaluation scores.

``` r
ggplot(evals, aes(x = score, y = bty_avg))+
  geom_jitter()
```

![](lab-09_files/figure-gfm/jitter-1.png)<!-- -->

### Exercise 4

If we fit a linear regression with course evaluation scores predicted by
average beauty ratings, we get the following linear model:

score = 3.88 + .067(average beauty)

``` r
m_bty <- lm(data = evals, score ~ bty_avg)

summary(m_bty)
```

    ## 
    ## Call:
    ## lm(formula = score ~ bty_avg, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.9246 -0.3690  0.1420  0.3977  0.9309 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.88034    0.07614   50.96  < 2e-16 ***
    ## bty_avg      0.06664    0.01629    4.09 5.08e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5348 on 461 degrees of freedom
    ## Multiple R-squared:  0.03502,    Adjusted R-squared:  0.03293 
    ## F-statistic: 16.73 on 1 and 461 DF,  p-value: 5.083e-05

### Exercise 5

We can plot the slope of average beauty predicting course evaluation
scores, which shows the slight positive linear relationship between
course evals and beauty that we just got from our model.

``` r
ggplot(evals, aes(x = score, y = bty_avg))+
  geom_point()+
  geom_smooth(method = "lm", se = F, color = "orange", linewidth = 2)+
  theme_classic()
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](lab-09_files/figure-gfm/scatter-slope-1.png)<!-- -->

### Exercise 6

A 1-point difference in a professor’s average beauty rating is
predicted, on average, to be associated with a .07-point difference in
that professor’s average course evaluation score in the same direction.

### Exercise 7

In this model, the intercept is 3.88, meaning that a professor with an
average beauty rating of 0 is predicted to have a course evaluation
score of 3.88. This intercept isn’t really interpretable, because the
beauty ratings were made on a scale of 1 - 10, so a rating of 0 is
meaningless in this context.

### Exercise 8

The R^2 value of the model is .035, meaning about 3.5% of the variance
in course evaluation scores is explained by average beauty ratings.

### Exercise 9

The linear model: score = 4.09 + .14(gender)

Slope of gender: Course evaluation ratings are predicted, on average, to
be .14 points higher for male course instructors compared to female
instructors.

Intercept: Female instructors are predicted, on average, to have a
course evaluation score of about 4.09.

``` r
m_gen <- lm(data = evals, score ~ gender)

summary(m_gen)
```

    ## 
    ## Call:
    ## lm(formula = score ~ gender, data = evals)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.83433 -0.36357  0.06567  0.40718  0.90718 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  4.09282    0.03867 105.852  < 2e-16 ***
    ## gendermale   0.14151    0.05082   2.784  0.00558 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5399 on 461 degrees of freedom
    ## Multiple R-squared:  0.01654,    Adjusted R-squared:  0.01441 
    ## F-statistic: 7.753 on 1 and 461 DF,  p-value: 0.005583

### Exercise 10

Linear model for female instructors: score = 4.09

Linear model for male instructors: score = 4.23

### Exercise 11

Linear model: score = 4.28 + -.13(tenure track) + -.15(tenured)

Intercept: Teaching faculty are predicted, on average, to have a course
evaluation score of about 4.28.

Slopes: Tenure-track faculty are predicted, on average, to have a course
evaluation score of about 4.15. Tenured faculty are predicted, on
average, to have a score of about 4.13.

``` r
m_rank <- lm(data = evals, score ~ rank)

summary(m_rank)
```

    ## 
    ## Call:
    ## lm(formula = score ~ rank, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.8546 -0.3391  0.1157  0.4305  0.8609 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       4.28431    0.05365  79.853   <2e-16 ***
    ## ranktenure track -0.12968    0.07482  -1.733   0.0837 .  
    ## ranktenured      -0.14518    0.06355  -2.284   0.0228 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5419 on 460 degrees of freedom
    ## Multiple R-squared:  0.01163,    Adjusted R-squared:  0.007332 
    ## F-statistic: 2.706 on 2 and 460 DF,  p-value: 0.06786

### Exercise 12

Let’s create a new version of the rank variable, with tenure track as
the base level.

``` r
evals <- evals %>%
  mutate(rank_relevel = case_when(
    rank == "tenure track" ~ "0",
    rank == "tenured" ~ "tenured",
    rank == "teaching" ~ "teaching"))
```

### Exercise 13

Linear model: score = 4.15 + .13(teaching) + -.02(tenured)

Intercept: Tenure-track faculty are predicted, on average, to have a
course evalutaion score of about 4.15

Slopes: Teaching faculty are predicted, on average, to have a course
evaluation score of about 4.28. Tenured faculty are predicted, on
average, to have a score of about 4.13.

The R^2 is .011, meaning that about 1.1% of the variance in evaluation
scores is explained by tenure status.

``` r
m_rank_relevel <- lm(data = evals, score ~ rank_relevel)

summary(m_rank_relevel)
```

    ## 
    ## Call:
    ## lm(formula = score ~ rank_relevel, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.8546 -0.3391  0.1157  0.4305  0.8609 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)           4.15463    0.05214  79.680   <2e-16 ***
    ## rank_relevelteaching  0.12968    0.07482   1.733   0.0837 .  
    ## rank_releveltenured  -0.01550    0.06228  -0.249   0.8036    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5419 on 460 degrees of freedom
    ## Multiple R-squared:  0.01163,    Adjusted R-squared:  0.007332 
    ## F-statistic: 2.706 on 2 and 460 DF,  p-value: 0.06786

### Exercise 14

Now let’s make a new variable that simply represents whether or not the
instructor is eligible for tenure, regardless of whether they are
currently tenured.

``` r
evals <- evals %>%
  mutate(tenure_eligible = if_else(rank == "teaching", "no", "yes"))
```

### Exercise 15

Linear model: score = 4.28 + -.14(tenure eligible)

Intercept: Faculty who are not tenure eligible are predicted, on
average, to have a course evaluation score of about 4.28.

Slope: Faculty who are tenure eligible are predicted, on average, to
have a course evaluation score of about 4.14.

The R^2 is .011, meaning 1.1% of the variance in evaluation scores is
explained by tenured status.

It’s also worth noting that the results and interpretations for
Exercises 11, 13, and 15 are completely consistent with each other,
which is good because they are all using the same data shaped in
slightly different variations.

``` r
m_tenure_eligible <- lm(data = evals, score ~ tenure_eligible)

summary(m_tenure_eligible)
```

    ## 
    ## Call:
    ## lm(formula = score ~ tenure_eligible, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.8438 -0.3438  0.1157  0.4360  0.8562 
    ## 
    ## Coefficients:
    ##                    Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          4.2843     0.0536  79.934   <2e-16 ***
    ## tenure_eligibleyes  -0.1406     0.0607  -2.315    0.021 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5413 on 461 degrees of freedom
    ## Multiple R-squared:  0.0115, Adjusted R-squared:  0.009352 
    ## F-statistic: 5.361 on 1 and 461 DF,  p-value: 0.02103
