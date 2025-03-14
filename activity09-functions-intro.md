Activity 9
================
Sajal

In this activity, we will create a function for calculating the range of
a numerical variable that is also flexible to other types of ranges
(e.g., the IQR).

## Setup

To begin, we need `{tidyverse}` and `{gapminder}` (for a dataset to test
our functions with) packages loaded.

``` r
library(tidyverse)
library("gapminder")
```

## Part I

### Calculating the range

In the space below, test the `max`, `min`, and `range` functions with
appropriate variables from the `gapminder` dataset using the
`dataset$variable` method (e.g., `function(dataset$variable)`).

``` r
max(gapminder$gdpPercap)
```

    ## [1] 113523.1

``` r
min(gapminder$gdpPercap)
```

    ## [1] 241.1659

``` r
range(gapminder$gdpPercap)
```

    ## [1]    241.1659 113523.1329

Now, recall that in statistics *range = max - min*. Below, describe how
you could you use the output from some combination of the above
functions to calculate the range? Note that I can think of two different
methods: 1) using `min` and `max`, and 2) using `range` to obtain the
two values, then taking the difference of these values using the square
bracket (`[]`) method (twice).

**Response**:

I will now walk through creating a function using the second method.
Whenever I write a function, I start with a specific example that works.

``` r
# see what an already existing function produces
range(gapminder$lifeExp)
```

    ## [1] 23.599 82.603

``` r
# pull larger value
range(gapminder$lifeExp)[2]
```

    ## [1] 82.603

``` r
# pull smaller value
range(gapminder$lifeExp)[1]
```

    ## [1] 23.599

``` r
# calculate statistical range
range(gapminder$lifeExp)[2] - range(gapminder$lifeExp)[1]
```

    ## [1] 59.004

Next, I will try to generalize the work. That is, I am trying to
determine which values/calculations “change” and which
values/calculations stay the same.

``` r
# set changing item
variable <- gapminder$lifeExp

# generalize previous example code
range_results <- range(variable)
larger_value <- range_results[2]
smaller_value <- range_results[1]

# perform calculation that stays the same as before
diff_values <- larger_value - smaller_value

# could also simply do
# diff_values <- range_results[2] - range_results[1]
```

Now I see that `gapminder$lifeExp` is the only thing that changes
whenever I want to calculate the range so I have an outline for my
function! Remember that a function has three main parts: 1) a name, 2)
arguments, and 3) a body.

    function_name <- function(function_arguments){
      function_body
    }

We will need to provide meaningful name that is different from “`range`”
since this is an object that already exists in R - remember namespace
issues. I will call this `diff_range`. Below, create our new range
function that, when supplied with a variable/vector `x`, returns the
statistical range.

``` r
diff_range <- function(x){
  # set changing values
  range_results <- range(x)
  larger_value <- range_results[2]
  smaller_value <- range_results[1]
  
  # perform calculation that stays the same
  diff_values <- larger_value - smaller_value
  return(diff_values)
}
```

Test this function to verify that it works. Below check this function
using the `lifeExp` column of the `gapminder` dataset. Also, verify that
it gets the correct result for `1:10` (a sequence of numbers starting at
1 and ending at 10, increasing by 1), `5:100`, `gapminder$pop` (the
population of each country in the `gapminder` data set), and
`gapminder$gdpPercap`.

``` r
diff_range(gapminder$lifeExp)
```

    ## [1] 59.004

``` r
if(diff_range(1:10) == 9) {
  print("test pass: 1")
}
```

    ## [1] "test pass: 1"

``` r
if(diff_range(5:500) == 495) {
  print("test pass: 2")
}
```

    ## [1] "test pass: 2"

``` r
diff_range(gapminder$pop)
```

    ## [1] 1318623085

``` r
diff_range(gapminder$gdpPercap)
```

    ## [1] 113282

### Your turn

Instead of using `min`, `max`, or `range`, create a function that
calculates the statistical range of a quantitative variable using the
`quantile` function. Note that the max (100th quantile) and min (0th
quantile) are special cases of quantiles. First, explore the quantile
function using `lifeExp`. Remember to use the `?` method to explore a
function’s help documentation. Convince yourself that you know how to
use it by being able to explain what the following calls return:

-   `quantile(gapminder$lifeExp)`
-   `quantile(gapminder$lifeExp, probs = 0.5)`
-   `quantile(gapminder$lifeExp, probs = c(0.25, 0.75))`

Also, explore this function with different `probs`.

``` r
quantile(gapminder$lifeExp)
```

    ##      0%     25%     50%     75%    100% 
    ## 23.5990 48.1980 60.7125 70.8455 82.6030

``` r
quantile(gapminder$lifeExp, probs = 0.5)
```

    ##     50% 
    ## 60.7125

``` r
quantile(gapminder$lifeExp, probs = c(0.25, 0.75))
```

    ##     25%     75% 
    ## 48.1980 70.8455

Once, you feel comfortable with the `quantile` function, create a
function named `quantile_range` that calculates the statistical range.

``` r
quantile_range <- function(x) {
  # set changing values
  results <- quantile(x)
  larger_value <- results[length(results)]
  smaller_value <- results[1]
  
  # perform calculation that stays the same
  diff_values <- larger_value - smaller_value
  
  names(diff_values) <- NULL
  
  return(diff_values)
}
```

Test your function to verify that it works. Below check this function
using the `lifeExp` column of the `gapminder` dataset. Also, verify that
it gets the correct result for `1:10` (a sequence of numbers starting at
1 and ending at 10, increasing by 1), `5:100`, `gapminder$pop` (the
population of each country in the `gapminder` data set), and
`gapminder$gdpPercap`.

``` r
quantile_range(gapminder$lifeExp)
```

    ## [1] 59.004

``` r
quantile_range(1:10)
```

    ## [1] 9

``` r
quantile_range(gapminder$pop)
```

    ## [1] 1318623085

``` r
quantile_range(gapminder$gdpPercap)
```

    ## [1] 113282

![](README-img/noun_pause.png) **Planned Pause Point**: This is the end
of Part 1 of this activity. You can use the rest of the class period
working on your project. The rest of this activity will be completed in
our next class session/at home.

## Part II

### Break the function

When we write functions, we should also make sure that it fails when we
expect it to fail, such as when provided with:

-   `gapminder` - an entire dataset
-   `gapminder$country` - a factor variable (these are stored as
    integers, right?)
-   `"Go Lakers!"` - a string

Below, check that our function breaks for each of the above tests.

For functions that will be used again, it is a good practice to add
argument validity checks.

-   `stopifnot` is a way to make sure the function arguments are of a
    specific type.
-   `if` then `stop` is a way to have more control over your error
    messages. Your instructor prefers to do this method because of the
    extra control (and ability to provide more descriptive error
    messages).

<!-- -->

    ...
      if(condition){
        stop("Descriptive message")
      }
    ...

Explore the two argument validity checks by adding them to the beginning
of your function above (one, then remove it and add the other) to check
if the input `x` is a numeric vector (`is.numeric`).

### Extend your function

Calculating the range of a set of data is convenient, but it would be
nice if we could determine the range between certain quantiles too. For
example, the interquartile range (*IQR = Q3 − Q1*).

``` r
quantile_range <- function(x, probs) {
  if(!(is.vector(x) && is.numeric(x))) {
    stop("first argument must be a vector of numeric type")
  }
  
  if(!(is.vector(probs) && is.numeric(probs))) {
    stop("probs argument must be a vector of numeric type")
  }
  
  # set changing values
  results <- quantile(x, probs)
  larger_value <- results[length(results)]
  smaller_value <- results[1]
  
  # perform calculation that stays the same
  diff_values <- larger_value - smaller_value
  
  names(diff_values) <- NULL
  
  return(diff_values)
}
```

Update your function so that it has the arguments `x` and `probs` and
calculates the differences between two specified quantiles supplied to
the `probs` argument. Test your function with:

-   IQR: `quantile_range(gapminder$lifeExp, probs = c(0.25, 0.75))`
-   RANGE: `quantile_range(gapminder$lifeExp, probs = c(0, 1))`

Also test that your function breaks when it should.

``` r
quantile_range(gapminder$lifeExp, probs = c(0.25, 0.75))
```

    ## [1] 22.6475

``` r
quantile_range(gapminder$lifeExp, probs = c(0, 1))
```

    ## [1] 59.004

``` r
quantile_range(gapminder$lifeExp, probs = c("a", "b"))
```

    ## Error in quantile_range(gapminder$lifeExp, probs = c("a", "b")): probs argument must be a vector of numeric type

Since you we using the `quantile` function in the `quantile_range`
function, we should be consistent with the arguments that we specify.
This is not necessary, but is extremely beneficial for humans to read,
write, and use the code. If you write functions based on already
existing functions, use a similar naming scheme.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Default values

What happens if you call your `quantile_range` function, but do not
supply specific probabilities? That is, check
`quantile_range(gapminder$lifeExp)`.

It is nice to provide some reasonable default values for certain
arguments. For `quantile_range`, it does not make since to specify a
default value for the primary input (`x`), but default values for
`probs` would be helpful. Since we began by calculating the difference
of the `max` and `min`, we could provide these as our default values.
Default values get set in the function definition (i.e.,
`function_name <- function(x, probs = c(...)`).

Update your `quantile_range` function below (copy, paste, edit) to have
the default values for the minimum and maximum. Check that your function
works by specifying and not specifying the `probs` argument. Then,
upgrade your function to contain a validity check for the new `probs`
argument.

``` r
quantile_range <- function(x, probs = c(0, 1), ...) {
  if(!(is.vector(x) && is.numeric(x))) {
    stop("first argument must be a vector of numeric type")
  }
  
  if(!(is.vector(probs) && is.numeric(probs))) {
    stop("probs argument must be a vector of numeric type")
  }
  
  # set changing values
  results <- quantile(x, probs, ...)
  larger_value <- results[length(results)]
  smaller_value <- results[1]
  
  # perform calculation that stays the same
  diff_values <- larger_value - smaller_value
  
  names(diff_values) <- NULL
  
  return(diff_values)
}

quantile_range(gapminder$lifeExp)
```

    ## [1] 59.004

### Handling `NA`s

Run the following code and discuss what you notice. What does
`na.rm = TRUE` do? What is the default value?

``` r
z <- gapminder$lifeExp
z[3] <- NA

quantile(gapminder$lifeExp)
```

    ##      0%     25%     50%     75%    100% 
    ## 23.5990 48.1980 60.7125 70.8455 82.6030

``` r
quantile(z)
```

    ## Error in quantile.default(z): missing values and NaN's not allowed if 'na.rm' is FALSE

``` r
quantile(z, na.rm = TRUE)
```

    ##     0%    25%    50%    75%   100% 
    ## 23.599 48.228 60.765 70.846 82.603

**Response:** `na.rm = TRUE` removes the null values. The default is
`FALSE`

Setting `na.rm` within your function without providing users a way to
override this behavior would be ill-advised. Update your `quant_diff`
function to be more robust (i.e., handle `NA`s) while also providing
users to control the behavior around `NA`s. You are welcome to enforce
your preferred choice when defining the default behavior, but be sure to
provide a way for users to control this.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### The `...` argument

Did you know that there is more than one way to compute quantiles!?!
Look at the `?quantile` help documentation and see that there are at
least nine types included. Essentially, if the quantile is not a equal
to an observed data value, some type of an average of two values will be
used.

Now that you know this, it would be nice to provide this and other
options within our function, but we do not want to clutter our
function’s interface. The `...` argument (an ellipsis) will help here.

After the `na.rm = (your_default_value)`, add `, ...` to your function
definition (literally, type `...` as the last argument on the first line
of your function). You will also need to add `...` to where you want to
allow for additional arguments (i.e., at the end of the `quantile`
function). Test that this worked by running the following lines of code:

``` r
quantile_range(gapminder$lifeExp, probs = c(0.25, 0.75), type = 1)
```

    ## [1] 22.686

``` r
quantile_range(gapminder$lifeExp, probs = c(0.25, 0.75), type = 4)
```

    ## [1] 22.686

The `...` argument is useful for when you want to provide the ability to
pass arbitrary arguments to another function, but don’t want to
constantly be expanding the formal arguments. There are downsides to
`...`:

-   For packages, you will have to create more informative documentation
    for your users;
-   It is quiet and absorbs other named arguments (e.g., if a user has a
    typo)

For more information, see the [ellipsis
package](https://ellipsis.r-lib.org/) documentation or the [tidyverse
design guide](https://principles.tidyverse.org/dots-position.html).

### Testing your function in the `{tidyverse}`

You have a function that is extremely flexible. Now we will see if it
works within the `{tidyverse}`.

In the code chunk below, calculate the IQR for the variable `gdpPercap`
in the gapminder dataset for each continent in the year 2002.

``` r
gapminder %>%
  filter(year==2002) %>%
  group_by(continent) %>%
  summarise(quantile_gdp = quantile_range(gdpPercap, c(0.25, 0.75))) %>%
  knitr::kable()
```

| continent | quantile\_gdp |
|:----------|--------------:|
| Africa    |      2534.309 |
| Americas  |      3939.293 |
| Asia      |     17141.276 |
| Europe    |     18651.512 |
| Oceania   |      3748.977 |

### Unit tests

While we won’t be doing formal unit testing on functions, if you are
going to use a function a lot (and especially if it is going to be part
of a package), it is wise to use formal unit tests. The
[**testthat**](https://testthat.r-lib.org/) package provides an
excellent framework for doing this with an emphasis on automated unit
testing for the entire package.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

## Attribution

This activity is based on materials from Jenny Bryan’s [STAT
545](https://stat545.com/) course.
