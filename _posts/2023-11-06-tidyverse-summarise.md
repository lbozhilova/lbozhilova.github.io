---
title:  "Learning to `summarise(across())` variables"
categories:
  - Blog
tags:
  - tidyverse
  - dplyr
  - R
  - software
---

In the past month I have been doing a fair bit of data engineering at work. As part of this I have had to generate summary tables of the clinical data we have, to go in reports, slides, and so on. Within [tidyverse](https://www.tidyverse.org/), the master tool for this job is `dplyr::summarise()`. It is magic, and can do virtually anything. 

Before I show you how, let us generate some toy data. The code below will generate a dataframe `toy_df` of 50,000 delivery records across two hospitals, `A` and `B`. Each record includes three logical variables: whether labour was induced `induced`, whether oxytocin was administered `oxyt`, and whether the delivery was operative, as opposed to spontaneous `opdel`. Some of the data is missing.

```
library("tidyverse")

toy_df <- data.frame(
  hospital = rep(c("A", "B"), times = c(2e4, 3e4)),
  induced  = as.logical(rbinom(5e4, 1, prob = rep(c(.35, .25), times = c(2e4, 3e4)))), 
  oxyt     = as.logical(rbinom(5e4, 1, prob = rep(c(.15, .10), times = c(2e4, 3e4)))), 
  opdel    = as.logical(rbinom(5e4, 1, prob = rep(c(.15, .20), times = c(2e4, 3e4))))) %>% 
  mutate(across(induced:opdel, function(x) { idx <- sample.int(length(x), floor(length(x) * .1)); x[idx] <- NA; x }))
```

Suppose that we were interested in summarising the data by hospital and counting the number of NAs for each clinical variable, as well as the proportion of positives. For clarity, let us define these as functions.

```
num_na    <- function(x) sum(is.na(x))
prop_true <- function(x) sum(x, na.rm = TRUE) / sum(!is.na(x))
```

The most basic way to `summarise()` is a little like `mutate()`, by defining a new summary variable:
```
toy_df %>% 
  group_by(hospital) %>% 
  summarise(induced_na = num_na(induced),
            oxyt_na    = num_na(oxyt))
```            
will produce a tibble like so
```
# A tibble: 2 × 3
  hospital induced_na oxyt_na
  <chr>         <int>   <int>
1 A              2015    2012
2 B              2985    2988
``` 

If we wanted to apply the same summary function `num_na()` to multiple columns, we can use `across()` inside the `summary()` call. This is useful when we have a large number of columns we want to summarise in the same way.

```
toy_df %>% 
  group_by(hospital) %>% 
  summarise(across(induced:opdel, num_na))
```
```
# A tibble: 2 × 4
  hospital induced  oxyt opdel
  <chr>      <int> <int> <int>
1 A           2015  2012  2030
2 B           2985  2988  2970
```

The function `across()` takes two main inputs: column names, and either a single function, or a list of functions, to apply to each of these columns. This means we can calculate both `num_na()` and `prop_true()` across all three columns in the same summarise call, like so:
```
toy_df %>% 
  group_by(hospital) %>% 
  summarise(across(induced:opdel, list("na" = num_na, "true" = prop_true)))
```
```
# A tibble: 2 × 7
  hospital induced_na induced_true oxyt_na oxyt_true opdel_na opdel_true
  <chr>         <int>        <dbl>   <int>     <dbl>    <int>      <dbl>
1 A              2015        0.351    2012    0.150      2030      0.155
2 B              2985        0.254    2988    0.0997     2970      0.197
```

As you can see, the summary variable names are a combination of the original column names and the function list names. If the list was not named (i.e. using `list(num_na, prop_true)` above), numeric suffixes would be used: `induced_1, induced_2`, etc. I recommend always using named lists both for clarity and because it makes it easier to then `pivot_longer()` if you like.

```
toy_df %>% 
  group_by(hospital) %>% 
  summarise(across(induced:opdel, list("na" = num_na, "true" = prop_true))) %>% 
  pivot_longer(induced_na:opdel_true, names_to = c("variable", "summary"), names_sep = "_")
```
```
# A tibble: 12 × 4
   hospital var     summary     value
   <chr>    <chr>   <chr>       <dbl>
 1 A        induced na      2015     
 2 A        induced true       0.351 
 3 A        oxyt    na      2012     
 4 A        oxyt    true       0.150 
 5 A        opdel   na      2030     
 6 A        opdel   true       0.155 
 7 B        induced na      2985     
 8 B        induced true       0.254 
 9 B        oxyt    na      2988     
10 B        oxyt    true       0.0997
11 B        opdel   na      2970     
12 B        opdel   true       0.197
```

What if we wanted different summaries for the different columns? We can use multiple `across()` calls in the same `summary()`. To prevent (not always) silent errors due to overwriting variable names, I strongly suggest sticking to the named list syntax. The following will calculate the proportion of positives for all three clinical variables, and the number of missing entries for `induced` and `oxyt` only.

```
toy_df %>% 
  group_by(hospital) %>% 
  summarise(across(induced:opdel, list("true" = prop_true)),
            across(induced:oxyt , list("na" = num_na))) 
```
```
# A tibble: 2 × 6
  hospital induced_true oxyt_true opdel_true induced_na oxyt_na
  <chr>           <dbl>     <dbl>      <dbl>      <int>   <int>
1 A               0.351    0.150       0.155       2015    2012
2 B               0.254    0.0997      0.197       2985    2988
```

Generating summary data tables is not the most glamorous part of science, but there are plenty of `dplyr` tricks to make it pleasingly quick and painless. If you want to dig deeper, I recommend looking through [the documentation](https://dplyr.tidyverse.org/reference/across.html). For example, instead of listing the columns explicitly `induced:opdel`, you can apply column filters such as `where(is.logical)` or `starts_with("o")`. 

Happy coding!
