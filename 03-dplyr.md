---
layout: topic
title: Manipulating and analyzing data with dplyr; Exporting data
author: Data Carpentry contributors
---





------------

> ### Learning Objectives
>
> * Understand the purpose of the **`dplyr`** and **`tidyr`** packages.
> * Select certain columns in a data frame with the **`dplyr`** function `select`.
> * Select certain rows in a data frame according to filtering conditions with the **`dplyr`** function `filter` .
> * Link the output of one **`dplyr`** function to the input of another function with the 'pipe' operator `%>%`.
> * Add new columns to a data frame that are functions of existing columns with `mutate`.
> * Understand the split-apply-combine concept for data analysis.
> * Use `summarize`, `group_by`, and `tally` to split a data frame into groups of observations, apply summary statistics for each group, and then combine the results.
> * Export a data frame to a .csv file.


------------

# Data Manipulation using **`dplyr`** and **`tidyr`**

Bracket subsetting is handy, but it can be cumbersome and difficult to read,
especially for complicated operations. 

Enter **`dplyr`**. 

**`dplyr`** is a package for
making tabular data manipulation easier. It pairs nicely with **`tidyr`**, which enables you to swiftly convert between different data formats for plotting and analysis.

Packages in R are basically sets of additional functions that let you do more
stuff in R. The functions we've been using so far, like `str()` or `data.frame()`,
come built into R; packages give you access to more functions. Before you use a
package for the first time, you need to install it on your machine, and then you
should load the package in every subsequent R session when you need it. You should
already have installed the **`tidyverse`** package. This is an
"umbrella-package" that installs several packages useful for data analysis which
work together well, such as **`tidyr`**, **`dplyr`**, **`ggplot2`**, etc. To load
the package type:



```r
library("tidyverse")    ## load the tidyverse packages, incl. dplyr
```

## What are **`dplyr`** and **`tidyr`**?

The package **`dplyr`** provides easy tools for the most common data manipulation
tasks. It is built to work directly with data frames, with many common tasks
optimized by being written in a compiled language (C++). An additional feature is the
ability to work directly with data stored in an external database. The benefits of
doing this are that the data can be managed natively in a relational database,
queries can be conducted on that database, and only the results of the query are
returned.

This addresses a common problem with R in that all operations are conducted
in-memory and thus the amount of data you can work with is limited by available
memory. The database connections essentially remove that limitation in that you
can have a database of many 100s GB, conduct queries on it directly, and pull
back into R only what you need for analysis.

The package **`tidyr`** addresses the common problem of wanting to reshape your data for plotting and use by different R functions. Sometimes we want data sets where we have one row per measurement. Sometimes we want a data frame where each measurement type has its own column, and rows are instead more aggregated groups - like plots or aquaria. Moving back and forth between these formats is nontrivial, and **`tidyr`** gives you tools for this and more sophisticated  data manipulation.

To learn more about **`dplyr`** and **`tidyr`** after the workshop, you may want to check out this
[handy data transformation with **`dplyr`** cheatsheet](https://github.com/rstudio/cheatsheets/raw/master/source/pdfs/data-transformation-cheatsheet.pdf) and this [one about **`tidyr`**](https://github.com/rstudio/cheatsheets/blob/master/source/pdfs/data-import-cheatsheet.pdf).


## Selecting columns and filtering rows

We're going to learn some of the most common **`dplyr`** functions: `select()`,
`filter()`, `mutate()`, `group_by()`, and `summarize()`. To select columns of a
data frame, use `select()`. The first argument to this function is the data
frame (`metadata`), and the subsequent arguments are the columns to keep.


```r
select(metadata, SampleID, Sex, Genotype)
```

To choose rows based on a specific criteria, use `filter()`:


```r
filter(metadata, Genotype == 'WT')
```

## Pipes

But what if you wanted to select and filter at the same time? There are three
ways to do this: use intermediate steps, nested functions, or pipes.

With intermediate steps, you essentially create a temporary data frame and use
that as input to the next function. This can clutter up your workspace with lots
of objects. You can also nest functions (i.e. one function inside of another).
This is handy, but can be difficult to read if too many functions are nested as
things are evaluated from the inside out.

The last option, pipes, are a fairly recent addition to R. Pipes let you take
the output of one function and send it directly to the next, which is useful
when you need to do many things to the same dataset.  Pipes in R look like
`%>%` and are made available via the `magrittr` package, installed automatically
with **`dplyr`**. If you use RStudio, you can type the pipe with <kbd>Ctrl</kbd>
+ <kbd>Shift</kbd> + <kbd>M</kbd> if you have a PC or <kbd>Cmd</kbd> + 
<kbd>Shift</kbd> + <kbd>M</kbd> if you have a Mac.


```r
metadata %>%
  filter(Genotype == 'WT') %>%
  select(SampleID, Sex, Genotype)
```

In the above, we use the pipe to send the `metadata` dataset first through
`filter()` to keep rows where `Genotype` is equeal to Wild Type, then through `select()`
to keep only the `SampleID`, `Sex`, and `Genotype` columns. Since `%>%` takes
the object on its left and passes it as the first argument to the function on
its right, we don't need to explicitly include it as an argument to the
`filter()` and `select()` functions anymore.

If we wanted to create a new object with this smaller version of the data, we
could do so by assigning it a new name:


```r
metadata_WT <- metadata %>%
  filter(Genotype == 'WT') %>%
  select(SampleID, Sex, Genotype)

metadata_WT
```

Note that the final data frame is the leftmost part of this expression.

> ### Challenge {.challenge}
>
>  Using pipes, subset the `metadata` to include rows where the Sex is male, and retain only the columns `SampleID` and `Genotype`.

<!---

```r
## Answer
metadata %>%
    filter(Sex == M) %>%
    select(SampleID, Genotype)
```
--->

    

### Mutate

Frequently you'll want to create new columns based on the values in existing
columns, for example to do unit conversions, or find the ratio of values in two
columns. For this we'll use `mutate()`.

To create a new column of Age in months:


```r
metadata %>%
  mutate(Weight_kg = Weight / 1000)
```

You can also create a second new column based on the first new column within the same call of `mutate()`:


```r
metadata %>%
  mutate(Weight_kg = Weight / 1000,
         Weight2 = Weight * 2)
```

If this runs off your screen and you just want to see the first few rows, you
can use a pipe to view the `head()` of the data. (Pipes work with non-**`dplyr`**
functions, too, as long as the **`dplyr`** or `magrittr` package is loaded).


```r
metadata %>%
  mutate(Weight_kg = Weight / 1000) %>%
  head
```

Note that we don't include parentheses at the end of our call to `head()` above.
When piping into a function with no additional arguments, you can call the
function with or without parentheses (e.g. `head` or `head()`).

The first few rows of the output are full of `NA`s, so if we wanted to remove
those we could insert a `filter()` in the chain:


```r
metadata %>%
  filter(!is.na(Weight)) %>%
  mutate(Weight_kg = Weight / 1000) %>%
  head
```

`is.na()` is a function that determines whether something is an `NA`. The `!`
symbol negates the result, so we're asking for everything that *is not* an `NA`.


### Split-apply-combine data analysis and the summarize() function

Many data analysis tasks can be approached using the *split-apply-combine*
paradigm: split the data into groups, apply some analysis to each group, and
then combine the results. **`dplyr`** makes this very easy through the use of the
`group_by()` function, which splits the data into groups. `group_by()` takes as arguments
the column names that contain the **categorical** variables for which you want
to calculate the summary statistics.


#### The `summarize()` function

`group_by()` is often used together with `summarize()`, which collapses each
group into a single-row summary of that group.  `summarize()` does this by applying an aggregating
or summary function to each group. For example, if we wanted to group by 
Sex and find the number of rows of data for each 
gender, we would do: 


```r
metadata %>%
  group_by(Sex) %>%
  summarize(n())
```

Here the summary function used was `n()` to find the count for each
group. We can also apply many other functions  to individual columns
to get other summary statistics.
For example, in the R base package we can use built-in functions like
`mean`, `median`, `min`, and `max`.  By default, all **R functions
operating on vectors that contains missing data will return NA**.
It's a way to make sure that users know they have missing
data, and make a conscious decision on how to deal with it. When
dealing with simple statistics like the mean, the easiest way to
ignore `NA` (the missing data) is to use `na.rm=TRUE` (`rm` stands for
remove).

So to view mean `Age` by Sex:


```r
metadata %>%
  group_by(Sex) %>%
  summarize(mean_weight = mean(Weight, na.rm = TRUE))
```

You may also have noticed that the output from these calls doesn't run off the
screen anymore. That's because **`dplyr`** has changed our `data.frame` object
to an object of class `tbl_df`, also known as a "tibble". Tibble's data
structure is very similar to a data frame. For our purposes the only differences
are that, (1) in addition to displaying the data type of each column under its
name, it only prints the first few rows of data and only as many columns as fit
on one screen, (2) columns of class `character` are never converted into
factors.

You can also group by multiple columns:


```r
metadata %>%
  group_by(Sex, Genotype) %>%
  summarize(mean_weight = mean(Weight, na.rm = TRUE))
```

When grouping both by `Sex` and `Genotype`, the first rows are for individuals
for which their Sex was not recorded. We can remove the missing values for weight before we
attempt to calculate the summary statistics. Because the missing
values are removed, we can omit `na.rm = TRUE` when computing the mean:


```r
metadata %>%
  filter(!is.na(Weight)) %>%
  group_by(Sex, Genotype) %>%
  summarize(mean_weight = mean(Weight))
```

Once the data are grouped, you can also summarize multiple variables at the same
time (and not necessarily on the same variable). For instance, we could add a
column indicating the minimum Weight for each Genotype for each Sex:


```r
metadata %>%
  filter(!is.na(Weight)) %>%
  group_by(Sex, Genotype) %>%
  summarize(mean_weight = mean(Weight),
            min_weight = min(Weight))
```


#### Tallying

When working with data, it is also common to want to know the number of
observations found for each factor or combination of factors. For this, **`dplyr`**
provides `tally()`. For example, if we wanted to group by Sex and find the
number of rows of data for each Sex, we would do:


```r
metadata %>%
  group_by(Sex) %>%
  tally
```

Here, `tally()` is the action applied to the groups created by `group_by()` and
counts the total number of records for each category.


# Exporting data

Now that you have learned how to use **`dplyr`** to extract information from
or summarize your raw data, you may want to export these new datasets to share
them with your collaborators or for archival.

Similar to the `read.csv()` function used for reading CSV files into R, there is
a `write.csv()` function that generates CSV files from data frames.

Before using `write.csv()`, we are going to create a new folder, `data_output`,
in our working directory that will store this generated dataset. We don't want
to write generated datasets in the same directory as our raw data. It's good
practice to keep them separate. The `data` folder should only contain the raw,
unaltered data, and should be left alone to make sure we don't delete or modify
it. In contrast, our script will generate the contents of the `data_output`
directory, so even if the files it contains are deleted, we can always
re-generate them.

In preparation for our next lesson on plotting, we are going to prepare a
cleaned up version of the dataset that doesn't include any missing data.

Remove observations for which the `Sex` is missing. In
this dataset, the missing Sex is represented by an empty string and not an
`NA`. Let's also remove observations for which `Weight` is missing. 


```r
metadata_complete <- metadata %>%
  filter(!is.na(Weight),           # remove missing Weight
         Sex != "")                # remove missing Sex
```


To make sure that everyone has the same dataset, check that `metadata_complete`
has 99 rows and 11 columns by
typing `dim(metadata_complete)`.

Now that our dataset is ready, we can save it as a CSV file in our `data_output`
folder. By default, `write.csv()` includes a column with row names (in our case
the names are just the row numbers), so we need to add `row.names = FALSE` so
they are not included:


```r
dir.create("data_output")

write.csv(metadata_complete, file = "data_output/metadata_complete.csv",
          row.names = FALSE)
```


<p style="text-align: right; font-size: small;">Page build on: 2017-10-17 09:49:00</p>
