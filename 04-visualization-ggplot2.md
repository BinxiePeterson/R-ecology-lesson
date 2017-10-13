---
title: Data visualization with ggplot2
author: Data Carpentry contributors
minutes: 60
---







------------

> ### Learning Objectives
>
> * Create simple scatterplots, histograms, and boxplots in R.
> * Set universal plot settings.
> * Understand and apply faceting in ggplot.
> * Modify the aesthetics of an existing ggplot plot (including axis labels and color).
> * Build complex and customized plots from data in a data frame.

--------------

We start by loading the required packages. **`ggplot2`** is included in the **`tidyverse`** package.


```r
library(ggplot2)
library(tidyverse)
```

If not still in the workspace, load the data we saved in the previous lesson.


```r
metadata <- read.csv('data/metadata.csv', header = TRUE, sep = "\t")
```


# Basic plots in R

The mathematician Richard Hamming once said, "The purpose of computing is insight, not numbers", and the best way to develop insight is often to visualize data. Visualization deserves an entire lecture (or course) of its own, but we can explore a few features of R's plotting packages.

When we are working with large sets of numbers it can be useful to display that information graphically. R has a number of built-in tools for basic graph types such as hisotgrams, scatter plots, bar charts, boxplots and much [more](http://www.statmethods.net/graphs/). We'll test a few of these out here on the `Age` vector from our metadata.



```r
metadata_age <- metadata$Age
```

```
#> Error in eval(expr, envir, enclos): object 'metadata' not found
```

## Scatterplot
Let's start with a **scatterplot**. A scatter plot provides a graphical view of the relationship between two sets of numbers. We don't have a variable in our metadata that is a continous variable, so there is nothing to plot it against but we can plot the values against their index values just to demonstrate the function.


```r
plot(metadata_age)
```

```
#> Error in plot(metadata_age): object 'metadata_age' not found
```

Each point represents a clone and the value on the x-axis is the clone index in the file, where the values on the y-axis correspond to the genome size for the clone. For any plot you can customize many features of your graphs (fonts, colors, axes, titles) through [graphic options](http://www.statmethods.net/advgraphs/parameters.html)
For example, we can change the shape of the data point using `pch`.


```r
plot(metadata_age, pch=8)
```

```
#> Error in plot(metadata_age, pch = 8): object 'metadata_age' not found
```

We can add a title to the plot by assigning a string to `main`:


```r
plot(metadata_age, pch=8, main="Scatter plot of age distribution")
```

```
#> Error in plot(metadata_age, pch = 8, main = "Scatter plot of age distribution"): object 'metadata_age' not found
```

## Histogram
Another way to visualize the distribution of ages is to use a histogram, we can do this buy using the `hist` function:


```r
hist(metadata_age)
```

```
#> Error in hist(metadata_age): object 'metadata_age' not found
```

## Boxplot

Using additional information from our metadata, we can use plots to compare values between the different body mass indices (BMIs) using a **boxplot**. A boxplot provides a graphical view of the median, quartiles, maximum, and minimum of a data set. 


```r
# Boxplot
boxplot(metadata_age ~ BodyMassIndex, metadata)
```

```
#> Error in eval(expr, envir, enclos): object 'metadata' not found
```

Similar to the scatterplots above, we can pass in arguments to add in extras like plot title, axis labels and colors.


```r
boxplot(metadata_age ~ BodyMassIndex, metadata,  col=c("pink","purple", "darkgrey"),
        main="Average expression differences between celltypes", ylab="Expression")
```

```
#> Error in eval(expr, envir, enclos): object 'metadata' not found
```



## Advanced figures (`ggplot2`)

More recently, R users have moved away from base graphic options and towards a plotting package called [`ggplot2`](http://docs.ggplot2.org/) that adds a lot of functionality to the basic plots seen above. The syntax takes some getting used to but it's extremely powerful and flexible. We can start by re-creating some of the above plots but using ggplot functions to get a feel for the syntax.

`ggplot2` is best used on data in the `data.frame` form, so we will will work with `metadata` for the following figures. Let's start by loading the `ggplot2` library.


```r
library(ggplot2)
```


The `ggplot()` function is used to initialize the basic graph structure, then we add to it. The basic idea is that you specify different parts of the plot, and add them together using the `+` operator. This helps in creating
publication quality plots with minimal amounts of adjustments and tweaking.

ggplot likes data in the 'long' format: i.e., a column for every dimension,
and a row for every observation. Well structured data will save you lots of time
when making figures with ggplot.

To build a ggplot, we need to:

- use the `ggplot()` function and bind the plot to a specific data frame using the  
      `data` argument


```r
ggplot(data = metadata)
```

Geometric objects are the actual marks we put on a plot. Examples include:

* points (`geom_point`, for scatter plots, dot plots, etc)
* lines (`geom_line`, for time series, trend lines, etc)
* boxplot (`geom_boxplot`, for, well, boxplots!)

A plot **must have at least one geom**; there is no upper limit. You can add a geom to a plot using the + operator


```r
ggplot(metadata) +
  geom_point() # note what happens here
```

Each type of geom usually has a **required set of aesthetics** to be set, and usually accepts only a subset of all aesthetics --refer to the geom help pages to see what mappings each geom accepts. Aesthetic mappings are set with the aes() function. Examples include:

* position (i.e., on the x and y axes)
* color ("outside" color)
* fill ("inside" color) shape (of points)
* linetype
* size

To start, we will add position for the x- and y-axis since `geom_point` requires mappings for x and y, all others are optional.



```r
ggplot(data = metadata) +
  geom_point(aes(x = Age, y= BodyMassIndex))
```

```
#> Error in ggplot(data = metadata): object 'metadata' not found
```

The same figure will be generated if the aesthetics are place in the ggplot() function:


```r
ggplot(data = metadata, aes(x = Age, y= BodyMassIndex)) +
  geom_point()
```

```
#> Error in ggplot(data = metadata, aes(x = Age, y = BodyMassIndex)): object 'metadata' not found
```

If labels on the x-axis are hard to read, you can add an additional theme layer. The ggplot2 `theme` system handles non-data plot elements such as:

* Axis labels
* Plot background
* Facet label backround
* Legend appearance

There are built-in themes we can use, or we can adjust specific elements. For demonstration purposes, let's change the x-axis labels to be plotted on a 45 degree angle with a small horizontal shift. We will also add some additional aesthetics by mapping them to other variables in our dataframe. _For example, the color of the points will reflect the number of generations and the shape will reflect citrate mutant status._ The size of the points can be adjusted within the `geom_point` but does not need to be included in `aes()` since the value is not mapping to a variable.



```r
ggplot(data = metadata, aes(x = Age, y= BodyMassIndex, color = BodySuperSite, shape = Sex)) +
  geom_point(size = rel(3.0)) +
  theme(axis.text.x = element_text(angle=45, hjust=1))
```

```
#> Error in ggplot(data = metadata, aes(x = Age, y = BodyMassIndex, color = BodySuperSite, : object 'metadata' not found
```


## Histogram

To plot a histogram we require another geometric object `geom_bar`, which requires a statistical transformation. Some plot types (such as scatterplots) do not require transformations, each point is plotted at x and y coordinates equal to the original value. Other plots, such as boxplots, histograms, prediction lines etc. need to be transformed, and usually has a default statistic that can be changed via the `stat_bin` argument. 


```r
ggplot(data = metadata, aes(x = Age)) +
  geom_bar()
```

Try plotting with the default value and compare it to the plot using the binwidth values. How do they differ?


```r
ggplot(data = metadata, aes(x = Age)) +
  geom_bar(stat = "bin", binwidth=0.05)
```

```
#> Error in ggplot(data = metadata, aes(x = Age)): object 'metadata' not found
```

## Boxplot

Now that we have all the required information, let's try plotting a boxplot similar to what we had done using the base plot functions at the start of this lesson. We can add some additional layers to include a plot title and change the axis labels. Explore the code below and all the different layers that we have added to understand what each layer contributes to the final graphic.


```r
ggplot(data = metadata, aes(x = Sex, y = BodyMassIndex)) +
  geom_boxplot() +
  ggtitle('Distribution of body mass index by sex') +
  xlab('Gender') +
  ylab('Body mass index') +
  theme(panel.grid.major = element_line(size = .5, color = "grey"),
          axis.text.x = element_text(angle=45, hjust=1),
          axis.title = element_text(size = rel(1.5)),
          axis.text = element_text(size = rel(1.25)))
```

```
#> Error in ggplot(data = metadata, aes(x = Sex, y = BodyMassIndex)): object 'metadata' not found
```


By adding points to boxplot, we can have a better idea of the number of
measurements and of their distribution:


```r
ggplot(data = metadata, aes(x = Sex, y = BodyMassIndex)) +
  geom_boxplot() +
  geom_jitter(alpha = 0.3, color = "tomato") +
  theme_bw() +
  ggtitle('Distribution of body mass index by sex') +
  xlab('Gender') +
  ylab('Body mass index') +
  theme(panel.grid.major = element_line(size = .5, color = "grey"),
          axis.text.x = element_text(angle=45, hjust=1),
          axis.title = element_text(size = rel(1.5)),
          axis.text = element_text(size = rel(1.25)))
```

```
#> Error in ggplot(data = metadata, aes(x = Sex, y = BodyMassIndex)): object 'metadata' not found
```


Notes:

- The `+` sign used to add layers must be placed at the end of each line containing
a layer. If, instead, the `+` sign is added in the line before the other layer,
**`ggplot2`** will not add the new layer and will return an error message.


## **`ggplot2`** themes

In addition to `theme_bw()`, which changes the plot background to white, **`ggplot2`**
comes with several other themes which can be useful to quickly change the look
of your visualization. The complete list of themes is available
at <http://docs.ggplot2.org/current/ggtheme.html>. `theme_minimal()` and
`theme_light()` are popular, and `theme_void()` can be useful as a starting
point to create a new hand-crafted theme.

The
[ggthemes](https://cran.r-project.org/web/packages/ggthemes/vignettes/ggthemes.html) package
provides a wide variety of options (including an Excel 2003 theme).
The [**`ggplot2`** extensions website](https://www.ggplot2-exts.org) provides a list
of packages that extend the capabilities of **`ggplot2`**, including additional
themes.


## Customization

Take a look at the [**`ggplot2`** cheat sheet](https://www.rstudio.com/wp-content/uploads/2016/11/ggplot2-cheatsheet-2.1.pdf), for ideas on how you can improve plots.


# Writing figures to a file

There are two ways in which figures and plots can be output to a file (rather than simply displaying on screen). The first (and easiest) is to export directly from the RStudio 'Plots' panel, by clicking on `Export` when the image is plotted. This will give you the option of `png` or `pdf` and selecting the directory to which you wish to save it to. 

The second option is to use R functions in the console, allowing you the flexibility to specify parameters to dictate the size and resolution of the output image. Some of the more popular formats include `pdf()`, `png()`, which are functions that initialize a plot that will be written directly to a file in the `pdf` or `png` format, respectively. Within the function you will need to specify a name for your image in quotes and the width and height. Specifying the width and height is optional, but can be very useful if you are using the figure in a paper or presentation and need it to have a particular resolution. Note that the default units for image dimensions are either pixels (for png) or inches (for pdf). To save a plot to a file you need to:

1. Initialize the plot using the function that corresponds to the type of file you want to make: `pdf("filename")`
2. Write the code that makes the plot.
3. Close the connection to the new file (with your plot) using `dev.off()`.


```r
# Let's make a new directory where figures can be stored:

dir.create("figures")

pdf("figures/boxplot.pdf")

ggplot(data = metadata, aes(x = Sex, y = BodyMassIndex)) +
  geom_boxplot() +
  geom_jitter(alpha = 0.3, color = "tomato") +
  theme_bw() +
  ggtitle('Distribution of body mass index by sex') +
  xlab('Gender') +
  ylab('Body mass index') +
  theme(panel.grid.major = element_line(size = .5, color = "grey"),
          axis.text.x = element_text(angle=45, hjust=1),
          axis.title = element_text(size = rel(1.5)),
          axis.text = element_text(size = rel(1.25)))

dev.off()
```

Resources:
---------
We have only scratched the surface here. To learn more, see the [ggplot2 reference site](http://docs.ggplot2.org/), and Winston Chang's excellent [Cookbook for R](http://wiki.stdout.org/rcookbook/Graphs/) site. Though slightly out of date, [ggplot2: Elegant Graphics for Data Anaysis](http://www.amazon.com/ggplot2-Elegant-Graphics-Data-Analysis/dp/0387981403) is still the definative book on this subject. Much of the material here was adpapted from [Introduction to R graphics with ggplot2 Tutorial at IQSS](http://tutorials.iq.harvard.edu/R/Rgraphics/Rgraphics.html).

<p style="text-align: right; font-size: small;">Page build on: 2017-10-10 09:43:28</p>