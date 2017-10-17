---
layout: topic
title: Introduction to R
author: Data Carpentry contributors
minutes: 45
---




------------

> ### Learning Objectives
>
> * Define the following terms as they relate to R: object, assign, call,
>   function, arguments, options.
> * Create objects and and assign values to them.
> * Use comments to inform script.
> * Do simple arithmetic operations in R using values and objects.
> * Call functions and use arguments to change their default options.
> * Inspect the content of vectors and manipulate their content.
> * Subset and extract values from vectors.
> * Correctly define and handle missing values in vectors.

------------


## Creating objects in R



You can get output from R simply by typing math in the console:


```r
3 + 5
12 / 7
```

We can also comment what it is we're doing


```r
# I am adding 3 and 5. R is fun!
3+5
```

## Comments

The comment character in R is `#`, anything to the right of a `#` in a script
will be ignored by R. It is useful to leave notes, and explanations in your
scripts.
RStudio makes it easy to comment or uncomment a paragraph: after selecting the
lines you  want to comment, press at the same time on your keyboard
<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd>. If you only want to comment
out one line, you can put the cursor at any location of that line (i.e. no need 
to select the whole line), then press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + 
<kbd>C</kbd>.

## Asignment operator

To do useful and interesting things, we need to assign _values_ to
_objects_. To create an object, we need to give it a name followed by the
assignment operator `<-`, and the value we want to give it:


```r
# Assign 3 to a
a <- 3
# Assign 5 to b
b <- 5

# What is the value of a?
a
# What is the value of b?
b

# Add a and b
a + b
```

`<-` is the assignment operator. It assigns values on the right to objects on
the left. So, after executing `a <- 3`, the value of `a` is `3`. The arrow can
be read as 3 **goes into** `a`.  For historical reasons, you can also use `=`
for assignments, but not in every context. Because of
the
[slight](http://blog.revolutionanalytics.com/2008/12/use-equals-or-arrow-for-assignment.html) [differences](https://web.archive.org/web/20130610005305/https://stat.ethz.ch/pipermail/r-help/2009-March/191462.html) in
syntax, it is good practice to always use `<-` for assignments.

In RStudio, typing <kbd>Alt</kbd> + <kbd>-</kbd> (push <kbd>Alt</kbd> at the
same time as the <kbd>-</kbd> key) will write ` <- ` in a single keystroke.

### Exercise 

- What happens if we change `a` and then re-add `a` and `b`?
- Does it work if you just change `a` in the script and then add `a` and `b`? Did you still get the same answer after they changed `a`? If so, why do you think that might be?


## Notes on objects

Objects can be given any name such as `x`, `current_temperature`, or
`subject_id`. You want your object names to be explicit and not too long. They
cannot start with a number (`2x` is not valid, but `x2` is). R is case sensitive
(e.g., `weight_kg` is different from `Weight_kg`). There are some names that
cannot be used because they are the names of fundamental functions in R (e.g.,
`if`, `else`, `for`,
see
[here](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Reserved.html)
for a complete list). In general, even if it's allowed, it's best to not use
other function names (e.g., `c`, `T`, `mean`, `data`, `df`, `weights`). If in
doubt, check the help to see if the name is already in use. It's also best to
avoid dots (`.`) within a variable name as in `my.dataset`. There are many
functions in R with dots in their names for historical reasons, but because dots
have a special meaning in R (for methods) and other programming languages, it's
best to avoid them. It is also recommended to use nouns for variable names, and
verbs for function names. It's important to be consistent in the styling of your
code (where you put spaces, how you name variables, etc.). Using a consistent
coding style makes your code clearer to read for your future self and your
collaborators. In R, three popular style guides
are
[Google's](https://google.github.io/styleguide/Rguide.xml),
[Jean Fan's](http://jef.works/R-style-guide/) and
the [tidyverse's](http://style.tidyverse.org/). The tidyverse's is very comprehensive
and may seem overwhelming at first. You can install the 
[**`lintr`**](https://github.com/jimhester/lintr) to automatically check for issues 
in the styling of your code.


When assigning a value to an object, R does not print anything. You can force R to print the value by using parentheses or by typing the object name:


```r
c <- a + b    # doesn't print anything
(c <- a + b)  # but putting parenthesis around the call prints the value of `c`
c          # and so does typing the name of the object
```


### Functions and their arguments

The other key feature of R are functions. These are R's built in capabilities. Functions are "canned scripts" that automate more complicated sets of commands
including operations assignments, etc. Many functions are predefined, or can be
made available by importing R *packages* (more on that later). A function
usually gets one or more inputs called *arguments*. Functions often (but not
always) return a *value*. A typical example would be the function `sqrt()`. The
input (the argument) must be a number, and the return value (in fact, the
output) is the square root of that number. Executing a function ('running it')
is called *calling* the function. An example of a function call is:


```r
b <- sqrt(a)
```

Here, the value of `a` is given to the `sqrt()` function, the `sqrt()` function
calculates the square root, and returns the value which is then assigned to
variable `b`. This function is very simple, because it takes just one argument.

The return 'value' of a function need not be numerical (like that of `sqrt()`),
and it also does not need to be a single item: it can be a set of things, or
even a dataset. We'll see that when we read data files into R.

Arguments can be anything, not only numbers or filenames, but also other
objects. Exactly what each argument means differs per function, and must be
looked up in the documentation (see below). Some functions take arguments which
may either be specified by the user, or, if left out, take on a *default* value:
these are called *options*. Options are typically used to alter the way the
function operates, such as whether it ignores 'bad values', or what symbol to
use in a plot.  However, if you want something specific, you can specify a value
of your choice which will be used instead of the default.

Let's try a function that can take multiple arguments: `round()`.


```r
round(3.14159)
```

```
#> [1] 3
```

Here, we've called `round()` with just one argument, `3.14159`, and it has
returned the value `3`.  That's because the default is to round to the nearest
whole number. If we want more digits we can see how to do that by getting
information about the `round` function.  We can use `args(round)` or look at the
help for this function using `?round`.


```r
args(round)
```

```
#> function (x, digits = 0) 
#> NULL
```


```r
?round
```

We see that if we want a different number of digits, we can
type `digits=2` or however many we want.


```r
round(3.14159, digits = 2)
```

```
#> [1] 3.14
```

If you provide the arguments in the exact same order as they are defined, you
don't have to name them:


```r
round(3.14159, 2)
```

```
#> [1] 3.14
```

And if you do name the arguments, you can switch their order:


```r
round(digits = 2, x = 3.14159)
```

```
#> [1] 3.14
```

It's good practice to put the non-optional arguments (like the number you're
rounding) first in your function call, and to specify the names of all optional
arguments.  If you don't, someone reading your code might have to look up the
definition of a function with unfamiliar arguments to understand what you're
doing.

### Objects vs. variables

What are known as `objects` in `R` are known as `variables` in many other
programming languages.  Depending on the context, `object` and `variable` can
have drastically different meanings.  However, in this lesson, the two words are
used synonymously.  For more information see:
https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Objects


### Exercise

We're going to work with genome lengths.
- Create a variable `genome_length_mb` and assign it the value `4.6`

## Solution


```r
(genome_length_mb <- 4.6)
genome_length_mb
```

### Exercise

Now that R has `genome_length_mb` in memory, we can do arithmetic with it. For
instance, we may want to convert this to the weight of the genome in 
picograms (for some reason). 978Mb = 1picogram. Divide the 
genome length in Mb by 978. 

### Solution


```r
genome_length_mb / 978.0
```

It turns out an E. coli genome doesn't weigh very much.

### Exercise

We can also change the variable's value by assigning it a new one. Say
we want to think about a human genome rather than E. coli. Change `genome_length_mb` to 3000 and figure out the weight of the human 
genome. 

### Solution


```r
genome_length_mb <- 3000.0
genome_length_mb / 978.0
```

This means that assigning a value to one variable does not change the values of
other variables.  For example, let's store the genome's weight in a variable.


```r
genome_weight_pg <- genome_length_mb / 978.0
```

and then change `genome_length_mb` to 100.


```r
genome_length_mb <- 100
```


### Exercise

What do you think is the current content of the object `genome_weight_pg`? 3.06 or 0.102?


## Vectors and data types



A vector is the most common and basic data type in R, and is pretty much
the workhorse of R. A vector is composed by a series of values, which can be
either numbers or characters. We can assign a series of values to a vector using
the `c()` function. For example we can create a vector of genome lengths:


```r
glengths <- c(4.6, 3000, 50000)
glengths
```

A vector can also contain characters:


```r
species <- c("ecoli", "human", "dog")
species
```

The quotes around "ecoli", "human", etc. are essential here. Without the quotes R
will assume there are objects called `ecoli`, `human` and `dog`. As these objects
don't exist in R's memory, there will be an error message.


There are many functions that allow you to inspect the content of a
vector. `length()` tells you how many elements are in a particular vector:


```r
length(glengths)
length(species)
```


You can also do math with whole vectors. For instance if we wanted to multiply the genome lengths of all the genomes in the list, we can do


```r
5 * glengths
```

or we can add the data in the two vectors together


```r
new_lengths <- glengths + glengths
new_lengths
```

This is very useful if we have data in different vectors that we 
want to combine or work with. 

An important feature of a vector, is that all of the elements are the same type of data.
The function `class()` indicates the class (the type of element) of an object:


```r
class(glengths)
class(species)
```

The function `str()` provides an overview of the structure of an object and its
elements. It is a useful function when working with large and complex
objects:


```r
str(glengths)
str(species)
```

You can use the `c()` function to add other elements to your vector:

```r
lengths <- c(glengths, 90) # adding at the end
lengths <- c(30, glengths) # adding at the beginning
lengths
```

What happens here is that we take the original vector `glengths`, and we are
adding another item first to the end of the other ones, and then another item at
the beginning. We can do this over and over again to build a vector or a
dataset. As we program, this may be useful to autoupdate results that we are
collecting or calculating.

An **atomic vector** is the simplest R **data type** and is a linear vector of a single type. Above, we saw 
2 of the 6 main **atomic vector** types  that R
uses: `"character"` and `"numeric"` (or `"double"`). These are the basic building blocks that
all R objects are built from. The other 4 **atomic vector** types are:

* `"logical"` for `TRUE` and `FALSE` (the boolean data type)
* `"integer"` for integer numbers (e.g., `2L`, the `L` indicates to R that it's an integer)
* `"complex"` to represent complex numbers with real and imaginary parts (e.g.,
  `1 + 4i`) and that's all we're going to say about them
* `"raw"` for bitstreams that we won't discuss further

You can check the type of your vector using the `typeof()` function and inputting your vector as the argument.

Vectors are one of the many **data structures** that R uses. Other important
ones are lists (`list`), matrices (`matrix`), data frames (`data.frame`),
factors (`factor`) and arrays (`array`).


> ### Challenge
>
>
> * We’ve seen that atomic vectors can be of type character,
>   numeric (or double), integer, and logical. But what happens if we try to mix these types in
>   a single vector?
> <!-- * _Answer_: R implicitly converts them to all be the same type -->
>
> * What will happen in each of these examples? (hint: use `class()`
>   to check the data type of your objects):
>
>     ```r
>     num_char <- c(1, 2, 3, 'a')
>     num_logical <- c(1, 2, 3, TRUE)
>     char_logical <- c('a', 'b', 'c', TRUE)
>     tricky <- c(1, 2, 3, '4')
>     ```
>
> * Why do you think it happens?
> <!-- * _Answer_: Vectors can be of only one data type. R tries to convert (coerce)
>   the content of this vector to find a "common denominator". -->
>
> * You've probably noticed that objects of different types get
>   converted into a single, shared type within a vector. In R, we
>   call converting objects from one class into another class
>   _coercion_. These conversions happen according to a hierarchy,
>   whereby some types get preferentially coerced into other
>   types. Can you draw a diagram that represents the hierarchy of how
>   these data types are coerced?
> <!-- * _Answer_: `logical -> numeric -> character <-- logical` -->



## Subsetting vectors

If we want to extract one or several values from a vector, we must provide one
or several indices in square brackets. For instance:


```r
animals <- c("mouse", "rat", "dog", "cat")
animals[2]
```

```
#> [1] "rat"
```

```r
animals[c(3, 2)]
```

```
#> [1] "dog" "rat"
```

We can also repeat the indices to create an object with more elements than the
original one:


```r
more_animals <- animals[c(1, 2, 3, 2, 1, 4)]
more_animals
```

```
#> [1] "mouse" "rat"   "dog"   "rat"   "mouse" "cat"
```

R indices start at 1. Programming languages like Fortran, MATLAB, Julia, and R start
counting at 1, because that's what human beings typically do. Languages in the C
family (including C++, Java, Perl, and Python) count from 0 because that's
simpler for computers to do.

### Conditional subsetting

Another common way of subsetting is by using a logical vector. `TRUE` will
select the element with the same index, while `FALSE` will not:


```r
genome_length_mb <- c(21, 34, 39, 54, 55)
genome_length_mb[c(TRUE, FALSE, TRUE, TRUE, FALSE)]
```

```
#> [1] 21 39 54
```

Typically, these logical vectors are not typed by hand, but are the output of
other functions or logical tests. For instance, if you wanted to select only the
values above 50:


```r
genome_length_mb > 50    # will return logicals with TRUE for the indices that meet the condition
```

```
#> [1] FALSE FALSE FALSE  TRUE  TRUE
```

```r
## so we can use this to select only the values above 50
genome_length_mb[genome_length_mb > 50]
```

```
#> [1] 54 55
```

You can combine multiple tests using `&` (both conditions are true, AND) or `|`
(at least one of the conditions is true, OR):


```r
genome_length_mb[genome_length_mb < 30 | genome_length_mb > 50]
```

```
#> [1] 21 54 55
```

```r
genome_length_mb[genome_length_mb >= 30 & genome_length_mb == 39]
```

```
#> [1] 39
```

Here, `<` stands for "less than", `>` for "greater than", `>=` for "greater than
or equal to", and `==` for "equal to". The double equal sign `==` is a test for
numerical equality between the left and right hand sides, and should not be
confused with the single `=` sign, which performs variable assignment (similar
to `<-`).

A common task is to search for certain strings in a vector.  One could use the
"or" operator `|` to test for equality to multiple values, but this can quickly
become tedious. The function `%in%` allows you to test if any of the elements of
a search vector are found:


```r
animals <- c("mouse", "rat", "dog", "cat")
animals[animals == "cat" | animals == "rat"] # returns both rat and cat
```

```
#> [1] "rat" "cat"
```

```r
animals %in% c("rat", "cat", "dog", "duck", "goat")
```

```
#> [1] FALSE  TRUE  TRUE  TRUE
```

```r
animals[animals %in% c("rat", "cat", "dog", "duck", "goat")]
```

```
#> [1] "rat" "dog" "cat"
```

> ### Challenge (optional){.challenge}
>
> * Can you figure out why `"four" > "five"` returns `TRUE`?



<!--

```r
## Answers
## * When using ">" or "<" on strings, R compares their alphabetical order. Here
##   "four" comes after "five", and therefore is "greater than" it.
```
-->


## Missing data

As R was designed to analyze datasets, it includes the concept of missing data
(which is uncommon in other programming languages). Missing data are represented
in vectors as `NA`.

When doing operations on numbers, most functions will return `NA` if the data
you are working with include missing values. This feature
makes it harder to overlook the cases where you are dealing with missing data.
You can add the argument `na.rm=TRUE` to calculate the result while ignoring
the missing values.


```r
ages <- c(2, 4, 4, NA, 6)
mean(ages)
max(ages)
mean(ages, na.rm = TRUE)
max(ages, na.rm = TRUE)
```

If your data include missing values, you may want to become familiar with the
functions `is.na()`, `na.omit()`, and `complete.cases()`. See below for
examples.



```r
## Extract those elements which are not missing values.
ages[!is.na(ages)]

## Returns the object with incomplete cases removed. The returned object is an atomic vector of type `"numeric"` (or `"double"`).
na.omit(ages)

## Extract those elements which are complete cases. The returned object is an atomic vector of type `"numeric"` (or `"double"`).
ages[complete.cases(ages)]
```
Recall that you can use the `typeof()` function to find the type of your atomic vector.

Now that we have learned how to write scripts, and the basics of R's data
structures, we are ready to start working with the mappingfile we have been
using in the other lessons, and learn about data frames.


<p style="text-align: right; font-size: small;">Page build on: 2017-10-17 09:48:49</p>
