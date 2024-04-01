# README

## Quarto

Quarto enables you to weave together content and executable code into a
finished document. To learn more about Quarto see <https://quarto.org>.

## Running Code

When you click the **Render** button a document will be generated that
includes both content and the output of embedded code. You can embed
code like this:

``` r
# Load necessary packages
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(dplyr)
library(ggplot2)
library(readr)

# Read data from csv file
vowel_data <- read.csv("data/vowel_data.csv")
View(vowel_data)

# Manipulate dataframe
vowel_data_summary <- vowel_data %>%
  group_by(vowel, language) %>%
  summarise(
    Avg_F1 = mean(f1_cent),
    Avg_F2 = mean(f2_cent),
    SD_F1 = sd(f1_cent),
    SD_F2 = sd(f2_cent),
    Trajectory_Length = sum(sqrt(diff(f1_cent)^2 + diff(f2_cent)^2))
  )
```

    `summarise()` has grouped output by 'vowel'. You can override using the
    `.groups` argument.

``` r
# Display plots

# Plot 1: Trajectory length as function of vowel and language
plot1 <- ggplot(vowel_data_summary, aes(x = vowel, y = Trajectory_Length, fill = language)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "trajectory length as a function of vowel and language", x = "vowel", y = "trajectory length") +
  theme_minimal()

# Plot 2: F1 as a function of vowel and language
plot2 <- ggplot(vowel_data_summary, aes(x = vowel, y = Avg_F1, fill = language)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "F1 as a function of vowel and language", x = "vowel", y = "Avg F1") +
  theme_minimal()

# Plot 3: F2 as a function of vowel and language
plot3 <- ggplot(vowel_data_summary, aes(x = vowel, y = Avg_F2, fill = language)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "F2 as a function of vowel and language", x = "vowel", y = "Avg F2") +
  theme_minimal()

# Print plots
print(plot1)
```

![](README_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
print(plot2)
```

![](README_files/figure-commonmark/unnamed-chunk-1-2.png)

``` r
print(plot3)
```

![](README_files/figure-commonmark/unnamed-chunk-1-3.png)

``` r
# Calculate trajectory length for each vowel
vowel_data_summary <- vowel_data %>%
  group_by(vowel) %>%
  summarise(
    Avg_F1 = mean(f1_cent),
    Avg_F2 = mean(f2_cent),
    Trajectory_Length = sum(sqrt(diff(f1_cent)^2 + diff(f2_cent)^2))
  )

# Plot trajectory length in F1/F2 vowel space
plot_trajectory <- ggplot(vowel_data_summary, aes(x = Avg_F1, y = Avg_F2, size = Trajectory_Length)) +
  geom_point() +
  scale_size_continuous(name = "trajectory length") +
  labs(title = "trajectory length in F1/F2 vowel space", x = "F1", y = "F2") +
  theme_minimal()

# Print plot
print(plot_trajectory)
```

![](README_files/figure-commonmark/unnamed-chunk-1-4.png)

You can add options to executable code like this

The `echo: false` option disables the printing of code (only output is
displayed).
