Programming Background
================

``` r
# Working directory is /Users/Blog but for some reason the render function is looking into the _Rmd folder

rmarkdown::render(
    '2022-06-03-programming-background.Rmd', 
    output_format = "github_document",
    output_dir = "../_posts",
    output_options = list(
      html_preview = FALSE
    )
)
```

## Background

Coming from a .NET background, using languages like R is a bit different
since there is no compilation step and, because of that, no type
checking. A part of me likes this since the type checking can be helpful
to find errors as early as possible in the development process.

However, with interpretable languages like R more and more tools are
becoming available that make it easier to spot similar types of errors.

While R isn’t a hard language to learn, in fact, I’d say it’s one of the
easier ones, getting started with it has become a lot easier. Since
Jupyter and RStudio have come a lot, getting started has become so easy
you can start coding with a Chromebook and a book on R.

## Example R Output

``` r
plot(ToothGrowth)
```

![](../imagesunnamed-chunk-4-1.png)<!-- -->