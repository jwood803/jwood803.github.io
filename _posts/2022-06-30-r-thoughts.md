R Thoughts
================

# R Thoughts

After doing some programming and a project in R, I feel I have a decent
amount of experience to go over some thoughts of the language as a
whole. Mostly, it’s a very nice language to use. The base packages has a
lot of what you would need to get started doing statistical analysis and
the CRAN network has a lot of packages that others have written to do
even more.

One of the coolest packages is probably the “tidyverse” package, which
includes several packages for data analysis, such as readr for reading
in files or dplyr for data manipulation. For example, dplyr can do a lot
in terms of data manipulation.

``` r
library(tidyverse)

ToothGrowth %>%
  as_tibble() %>%
  filter(length >= 10 & dose < 2.0)
```

Another nice thing about R is that is has a great IDE for it - R Studio.
This IDE does a lot including helping to knit R Markdown documents and
has a built-in visualizer for plots.