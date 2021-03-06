
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ggstatsplot: `ggplot2` Based Plots with Statistical Details

[![packageversion](https://img.shields.io/badge/Package%20version-0.0.1-orange.svg?style=flat-square)](commits/master)
[![Travis Build
Status](https://travis-ci.org/IndrajeetPatil/ggstatsplot.svg?branch=master)](https://travis-ci.org/IndrajeetPatil/ggstatsplot)
[![AppVeyor Build
Status](https://ci.appveyor.com/api/projects/status/github/IndrajeetPatil/ggstatsplot?branch=master&svg=true)](https://ci.appveyor.com/project/IndrajeetPatil/ggstatsplot)
[![Coverage
Status](https://img.shields.io/codecov/c/github/IndrajeetPatil/ggstatsplot/master.svg)](https://codecov.io/github/IndrajeetPatil/ggstatsplot?branch=master)
[![Licence](https://img.shields.io/badge/licence-GPL--3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![Project Status: Active - The project has reached a stable, usable
state and is being actively
developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)
[![Last-changedate](https://img.shields.io/badge/last%20change-2018--03--13-yellowgreen.svg)](/commits/master)
[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![minimal R
version](https://img.shields.io/badge/R%3E%3D-3.3.3-6666ff.svg)](https://cran.r-project.org/)
[![Github
Issues](http://githubbadges.herokuapp.com/IndrajeetPatil/ggstatsplot/issues.svg)](https://github.com/IndrajeetPatil/ggstatsplot/issues)
[![Pending
Pull-Requests](http://githubbadges.herokuapp.com/IndrajeetPatil/ggstatsplot/pulls.svg)](https://github.com/IndrajeetPatil/ggstatsplot/pulls)
<!-- [![Dependency Status](http://img.shields.io/gemnasium/IndrajeetPatil/ggstatsplot.svg)](https://gemnasium.com/IndrajeetPatil/ggstatsplot) -->

## Overview

ggstatsplot is an extension of `ggplot2` package for creating graphics
with details from statistical tests included in the plots themselves and
targeted primarily at behavioral sciences community to provide a
one-line code to produce information-rich figures. Currently, it
supports only the most common types of tests used in analysis
(**parametric**, **nonparametric**, and **robust** versions of
**t-tets/anova**, **correlation**, and **contingency tables** analyses).
Future versions will include other types of analyses as well.

## Installation

``` r
# You can get the development version from GitHub:
# install.packages("devtools")
devtools::install_github("IndrajeetPatil/ggstatsplot", dependencies = TRUE)
```

## Usage

Here are examples of the three main functions currently supported in
`ggstatsplot`:

  - `ggbetweenstats`

This function creates a violin plot for **between**-group or
**between**- condition comparisons with results from statistical tests
in the subtitle:

``` r
ggstatsplot::ggbetweenstats(data = iris, 
                            x = Species, 
                            y = Sepal.Length)
#> Reference:  Welch's ANOVA is used as a default. (Delacre, Leys, Mora, & Lakens, PsyArXiv, 2018).Note:  Bartlett's test for homogeneity of variances for factor Species : p-value =  < 0.001
```

![](man/figures/README-ggbetweenstats1-1.png)<!-- -->

Number of other arguments can be specified to make this plot even more
informative and, additionally, this function returns a `ggplot2` object
and thus any of the graphics layers can be further modified:

``` r
library(ggplot2)

ggstatsplot::ggbetweenstats(
  data = iris,
  x = Species,
  y = Sepal.Length,
  mean.plotting = TRUE,                           # whether mean for each group id to be displayed 
  type = "robust",                                # which type of test is to be run
  outlier.tagging = TRUE,                         # whether outliers need to be tagged
  outlier.label = Sepal.Width,                    # variable to be used for the outlier tag
  xlab = "Type of Species",                       # label for the x-axis variable
  ylab = "Attribute: Sepal Length",               # label for the y-axis variable
  title = "Dataset: Iris flower data set",        # title text for the plot
  caption = expression(                           # caption text for the plot 
    paste(italic("Note"), ": this is a demo")
    )
  ) +                                             # further modifcation outside of ggstatsplot
  ggplot2::coord_cartesian(ylim = c(3, 8)) + 
  ggplot2::scale_y_continuous(breaks = seq(3, 8, by = 1)) 
#> Note:  Bartlett's test for homogeneity of variances for factor Species : p-value =  < 0.001
```

![](man/figures/README-ggbetweenstats2-1.png)<!-- -->

Variant of this function `ggwithinstats` is currently under work.

  - `ggscatterstats`

This function creates a scatterplot with marginal
histograms/boxplots/density/ violin plots from `ggExtra::ggMarginal()`
and results from statistical tests in subtitle:

``` r
ggstatsplot::ggscatterstats(data = iris, 
                            x = Sepal.Length, 
                            y = Petal.Length,
                            title = "Dataset: Iris flower data set")
#> Warning: This function doesn't return ggplot2 object and is not further modifiable with ggplot2 commands.
```

![](man/figures/README-ggscatterstats1-1.png)<!-- -->

Number of other arguments can be specified to modify this basic plot-

``` r
library(datasets)

ggstatsplot::ggscatterstats(
  data = subset(iris, iris$Species == "setosa"),
  x = Sepal.Length,
  y = Petal.Length,
  test = "robust",                               # type of test that needs to be run
  xlab = "Attribute: Sepal Length",              # label for x axis
  ylab = "Attribute: Petal Length",              # label for y axis 
  line.colour = "green",                         # changing regression line colour line
  title = "Dataset: Iris flower data set",       # title text for the plot
  caption = expression(                          # caption text for the plot
    paste(italic("Note"), ": this is a demo")
    ),
  marginal.type = "density",                     # type of marginal distribution to be displayed
  xfill = "blue",                                # colour fill for x-axis marginal distribution 
  yfill = "red",                                 # colour fill for y-axis marginal distribution
  intercept = "median",                          # which type of intercept line is to be displayed  
  width.jitter = 0.2,                            # amount of horizontal jitter for data points
  height.jitter = 0.4                            # amount of vertical jitter for data points
  ) 
#> Note: Standardized robust regression using an M estimator: no. of iterations = 1000 In case of non-convergence, increase maxit value.Warning: This function doesn't return ggplot2 object and is not further modifiable with ggplot2 commands.
```

![](man/figures/README-ggscatterstats2-1.png)<!-- -->

**Important**: In contrast to all other functions in this package, the
`ggscatterstats` function returns object that is **not** further
modifiable with `ggplot2`. This can be avoided by not plotting the
marginal distributions (`marginal = FALSE`). Currently trying to find a
workaround this problem.

  - `ggpiestats`

This function creates a pie chart for categorical variables with results
from contingency table analysis included in the subtitle of the plot. If
only one categorical variable is entered, proportion test will be
carried out.

``` r
ggstatsplot::ggpiestats(data = iris,
                        main = Species)
#> Warning: No guarantee this function will work properly if you are using development version of ggplot2 (2.2.1.9000)
```

![](man/figures/README-ggpiestats1-1.png)<!-- -->

This function can also be used to study an interaction between two
categorical variables. Additionally, as with the other functions in
`ggstatsplot`, this function returns a `ggplot2` object and can further
be modified with `ggplot2` syntax (e.g., we can change the color palette
*after* `ggstatsplot` has produced the plot)-

``` r
library(ggplot2)

ggstatsplot::ggpiestats(data = mtcars,
                        main = am,                
                        condition = cyl) +
  ggplot2::scale_fill_brewer(palette = "Dark2")   # further modifcation outside of ggstatsplot    
#> Warning: No guarantee this function will work properly if you are using development version of ggplot2 (2.2.1.9000)
```

![](man/figures/README-ggpiestats2-1.png)<!-- -->

As with the other functions, this basic plot can further be modified
with additional arguments:

``` r
library(ggplot2)

ggstatsplot::ggpiestats(
  data = mtcars,
  main = am,
  condition = cyl,
  title = "Dataset: Motor Trend Car Road Tests",      # title for the plot
  stat.title = "interaction effect",                  # title for the results from Pearson's chi-squared test
  legend.title = "Transmission",                      # title for the legend
  factor.levels = c("0 = automatic", "1 = manual"),   # renaming the factor level names for main variable 
  facet.wrap.name = "No. of cylinders",               # name for the facetting variable
  caption = expression(                               # text for the caption
    paste(italic("Note"), ": this is a demo")
    )
) 
#> Warning: No guarantee this function will work properly if you are using development version of ggplot2 (2.2.1.9000)
```

![](man/figures/README-ggpiestats3-1.png)<!-- -->

  - `gghistostats`

In case you would like to see the distribution of one variable and check
if it is significantly different from a specified value with a one
sample test, this function will let you do that.

``` r
library(datasets)
library(viridis)

ggstatsplot::gghistostats(
  data = iris,
  x = Sepal.Length,
  title = "Distribution of Iris sepal length",
  type = "parametric",            # one sample t-test
  test.value = 3,                 # default value is 0
  centrality.para = "mean",       # which measure of central tendency is to be plotted
  centrality.colour = "red",      # decides colour of vertical line representing central tendency
  density.plot = TRUE,            # whether density plot is to be overlayed on a histogram
  binwidth.adjust = TRUE,         # whether binwidth needs to be adjusted
  binwidth = 0.10                 # binwidth value (needs to be toyed around with until you find the best one)
) +              
  viridis::scale_fill_viridis()   # further modifcation outside of ggstatsplot
#> Note:  Shapiro-Wilk test of normality for Sepal.Length : p-value =  0.010
```

![](man/figures/README-gghistostats-1.png)<!-- -->

  - `combine_plots`

`ggstatsplot` also contains a helper function `combine_plots` to combine
multiple plots. This is a wrapper function around `cowplot::plot_grid`
and lets you combine multiple plots and add combination of title,
caption, and annotation texts.

``` r
library(ggplot2)
library(plyr) 
library(glue)

ggstatsplot::combine_plots(
  plotlist = plyr::dlply(
    .data = iris,
    .variables = .(Species),
    .fun = function(data)
      ggstatsplot::ggscatterstats(
        data = data,
        x = Sepal.Length,
        y = Sepal.Width,
        marginal.type = "boxplot",
        title =
          glue::glue("Species: {(data$Species)} (n = {length(data$Sepal.Length)})")
      )
  ),
  labels = c("(a)", "(b)", "(c)"),
  nrow = 3,
  ncol = 1,
  title.text = "Relationship between sepal length and width for all Iris species",
  title.size = 14,
  title.colour = "blue",
  caption.text = expression(
    paste(
      italic("Note"),
      ": Iris flower dataset was collected by Edgar Anderson."
    ),
    caption.size = 10
  )
)
#> Warning: This function doesn't return ggplot2 object and is not further modifiable with ggplot2 commands.Warning: This function doesn't return ggplot2 object and is not further modifiable with ggplot2 commands.Warning: This function doesn't return ggplot2 object and is not further modifiable with ggplot2 commands.
```

![](man/figures/README-combine_plots_plyr-1.png)<!-- -->

The full power of this package can be leveraged with a functional
programming package like `purrr` that replaces many for loops with code
that is both more succinct and easier to read. Here is an example.
Notice how little code is needed not only to prepare the plots but also
to plot the statistical test results.

``` r
library(tidyverse)
library(glue)

### creating a list column with `ggstatsplot` plots
plots <- datasets::mtcars %>%
  dplyr::mutate(.data = ., cyl2 = cyl) %>%        # just creates a copy of this variable
  dplyr::group_by(.data = ., cyl) %>%             # 
  tidyr::nest(data = .) %>%                       # creates a nested dataframe with list column called `data`
  dplyr::mutate(                                  # creating a new list column of ggstatsplot outputs
    .data = .,
    plot = data %>%
      purrr::map(
        .x = .,
        .f = ~ ggstatsplot::ggbetweenstats(
          data = .,
          x = am,
          y = mpg,
          xlab = "Transmission",
          ylab = "Miles/(US) gallon",
          title = glue::glue(
            "Number of cylinders: {.$cyl2}"        # this is where the duplicated cyl2 column is useful
            ) 
        )
      )
  )
#> Warning:  aesthetic `x` was not a factor; converting it to factorReference:  Welch's t-test is used as a default. (Delacre, Lakens, & Leys, International Review of Social Psychology, 2017).Note:  Bartlett's test for homogeneity of variances for factor am : p-value =  0.317Warning:  aesthetic `x` was not a factor; converting it to factorReference:  Welch's t-test is used as a default. (Delacre, Lakens, & Leys, International Review of Social Psychology, 2017).Note:  Bartlett's test for homogeneity of variances for factor am : p-value =  0.144Warning:  aesthetic `x` was not a factor; converting it to factorReference:  Welch's t-test is used as a default. (Delacre, Lakens, & Leys, International Review of Social Psychology, 2017).Note:  Bartlett's test for homogeneity of variances for factor am : p-value =  0.201

### display the new object (notice that the class of the `plot` list column is S3: gg)
plots
#> # A tibble: 3 x 3
#>     cyl data               plot    
#>   <dbl> <list>             <list>  
#> 1    6. <tibble [7 x 11]>  <S3: gg>
#> 2    4. <tibble [11 x 11]> <S3: gg>
#> 3    8. <tibble [14 x 11]> <S3: gg>

### creating a grid with cowplot
ggstatsplot::combine_plots(plotlist = plots$plot,       # list column containing all ggstatsplot objects
                           nrow = 3,
                           ncol = 1,
                           labels = c("(a)","(b)","(c)"),
                           title.text = "MPG and car transmission relationship (for each cylinder count)",
                           title.size = 13,
                           title.colour = "blue",
                           caption.text = expression(
                             paste(
                               italic("Transmission"),
                               ": 0 = automatic, 1 = manual"
                             ),
                             caption.size = 10
                           ))
```

![](man/figures/README-combine_plots_purrr-1.png)<!-- -->

  - `theme_mprl`

`ggstatsplot` uses a default theme `theme_mprl()` that can be used with
any `ggplot2` objects.

``` r
library(ggplot2)

# Basic scatter plot
ggplot(mtcars, aes(x = wt, y = mpg)) + 
  geom_point()
```

![](man/figures/README-theme_mprl-1.png)<!-- -->

``` r
# Basic scatter plot with theme_mprl() added
ggplot(mtcars, aes(x = wt, y = mpg)) + 
  geom_point() + 
  ggstatsplot::theme_mprl()
```

![](man/figures/README-theme_mprl-2.png)<!-- -->

Please note that this project is released with a \[Contributor Code of
Conduct\] (.github/CODE\_OF\_CONDUCT.md). By participating in this
project you agree to abide by its terms.
