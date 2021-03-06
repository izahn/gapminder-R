---
layout: page
title: R for reproducible scientific analysis
subtitle: data.frame
minutes: 30
---




> ## Learning objectives {.objectives}
>
> - Be able to load a saved .rda object
> - Understand `data.frame` as common R object, composed of vectors
> - Understand basic R data classes
> - Be able to query the structure of an R object and its component parts.
> - Be able to subset a data.frame and vector
>


### Loading data 

We can load the dataset we just saved with the `load` function. Load needs the location of the saved file, provided as character string file-path, starting with the working directory. We won't get into working directories now, except to say that when your project's root directory will usually be your working directory. It is displayed at the top of your console pane in RStudio. File-paths should be provided relative to that location. So, to load the file we just saved:


~~~{.r}
load('data/continents.RDA')
~~~

### Intro to data.frames

Check your "Environment" tab -- a new object called `continents` has appeared. If your environment is in "list" mode, switch to "grid" as it's more informative. We can see that `continents` has Type data.frame and contains 6 observations of 4 variables. Click on the spreadsheet icon at far-right to view it. 

This is a `data.frame` -- the type of object most of us work with most of the time in R. In a `data.frame` each column represents a variable, and each row is an observation. This is the basic data organizational scheme -- one column per variable, one row per observation. You might recognize this form from from a statistics class or your own data analysis. 

#### Inspecting a data.frame

Rather than pulling up the spreadsheet form of a data.frame, we'd like to use R to get more information about it. In this case, our `data.frame` is so small that we can print the whole thing and inspect it:


~~~{.r}
continents
~~~



~~~{.output}
   continent area_km2 population percent_total_pop
1     Africa 30370000 1022234000              15.0
2   Americas 42330000  934611000              14.0
3 Antarctica 13720000       4490               0.0
4       Asia 43820000 4164252000              60.0
5     Europe 10180000  738199000              11.0
6    Oceania  9008500   29127000               0.4

~~~

When we start working with more-realistic datasets, let alone big data, that won't work. We can get the first few rows of a `data.frame` with `head` (in this case all the rows!). 


~~~{.r}
head(continents)
~~~



~~~{.output}
   continent area_km2 population percent_total_pop
1     Africa 30370000 1022234000              15.0
2   Americas 42330000  934611000              14.0
3 Antarctica 13720000       4490               0.0
4       Asia 43820000 4164252000              60.0
5     Europe 10180000  738199000              11.0
6    Oceania  9008500   29127000               0.4

~~~

`str` provides richer information about a `data.frame` and each element in it. It is generally a good first-step inspection of an R object.


~~~{.r}
str(continents)
~~~



~~~{.output}
'data.frame':	6 obs. of  4 variables:
 $ continent        : chr  "Africa" "Americas" "Antarctica" "Asia" ...
 $ area_km2         : num  30370000 42330000 13720000 43820000 10180000 ...
 $ population       : num  1.02e+09 9.35e+08 4.49e+03 4.16e+09 7.38e+08 ...
 $ percent_total_pop: num  15 14 0 60 11 0.4

~~~

We get some summary information on `continents`: it's type and dimensions, and we get some information on each variable in the `data.frame`. We'll get into the details of variable types later, but for now, note that `continent` is a character-type variable and the rest are numeric.

> #### Shoutout {.challenge}
>
> There is another function like `head()` and `str()` that provides information on a `data.frame`: `summary()`  
> - Call the `summary` function on the `continent` data.  
> - What is the average (mean) area of a continent?
>

### Vectors 

We can extract a single variable from a data.frame with `$` operator.


~~~{.r}
continents$population
~~~



~~~{.output}
[1] 1022234000  934611000       4490 4164252000  738199000   29127000

~~~

Note that you can use tab-completion to see what variables are available.

That just printed the six values of population. We are going to work with them some more, so extract them from the data.frame and store them to a new object, called `pop`:


~~~{.r}
pop <- continents$population
~~~

Now we have a new object in our environment: a numeric "vector" with six entries. R is built around vectors. In data analysis and statistics, we don't often work with individual numbers, but multiple observations. This is baked into R and helps it give it its power. 

Another core R concept is the idea that when you manipulate an object, the original object doesn't change. Note that the `continents` data.frame still has the population variable. R didn't "take it out" of `continents`; instead, it made a copy of it and stored it to a variable called `pop`. They are now two separate things. If we make a change to one, it will not affect the other. There is a powerful computational paradigm here that may not be apparent yet, but keep in mind that anything you do in R, if you want to keep the results, you need to assign them to a new object.

#### Vectorization

We just said that R is built around vectors. Most functions that accept a single value can also accept a vector of values. E.g. we can find the logarithm of each continent's population:


~~~{.r}
log(x = pop, base = 10)
~~~



~~~{.output}
[1] 9.009550 8.970631 3.652246 9.619537 8.868173 7.464296

~~~

R went over each item in `pop` and took the base-10 logarithm. Some functions take a vector but rather than returning a result for each item, operate on all of them together. E.g. we can find the mean and standard deviation of populations:


~~~{.r}
mean(pop)
~~~



~~~{.output}
[1] 1148071248

~~~



~~~{.r}
sd(pop)
~~~



~~~{.output}
[1] 1542519717

~~~

Many functions will also operate element-wise over multiple vectors. E.g. to calculate the population density of each continent, we need to divide the population by the land area for each continent. We can do that with a single command:


~~~{.r}
continents$population / continents$area_km2
~~~



~~~{.output}
[1] 3.365933e+01 2.207916e+01 3.272595e-04 9.503085e+01 7.251464e+01
[6] 3.233280e+00

~~~

Note that we didn't have to take those vectors out of the data.frame to use them. We can do vectorized operations right in the data.frame, which helps keep everything organized: recall that each row of a data.frame is a particular observation (in this case a continent), so we often want to do operations with each row.

Just like we can extract a column from a data.frame with `$`, we can make a new column:


~~~{.r}
continents$pop_density <- continents$population / continents$area_km2
continents
~~~



~~~{.output}
   continent area_km2 population percent_total_pop  pop_density
1     Africa 30370000 1022234000              15.0 3.365933e+01
2   Americas 42330000  934611000              14.0 2.207916e+01
3 Antarctica 13720000       4490               0.0 3.272595e-04
4       Asia 43820000 4164252000              60.0 9.503085e+01
5     Europe 10180000  738199000              11.0 7.251464e+01
6    Oceania  9008500   29127000               0.4 3.233280e+00

~~~


### Subsetting

#### Subsetting vectors 

R uses square brackets (`[ ]`) to subset data. To get the first element out of our `pop` vector, we do:


~~~{.r}
pop[1]
~~~



~~~{.output}
[1] 1022234000

~~~

To get the first three elements, we need to put 1, 2, and 3 inside the `[ ]`, but we need a way to group them together. The function "combine", `c` does that. This makes a vector of the numbers 1, 2, and 3:


~~~{.r}
c(1, 2, 3)
~~~



~~~{.output}
[1] 1 2 3

~~~

Then we can get the first three elements in `pop` like this:


~~~{.r}
pop[c(1, 2, 3)]
~~~



~~~{.output}
[1] 1022234000  934611000       4490

~~~

We can also tell R which elements we *don't* want with a `-`. This returns each element in `pop` except the first one:


~~~{.r}
pop[-1]
~~~



~~~{.output}
[1]  934611000       4490 4164252000  738199000   29127000

~~~

If we try to ask for an element that doesn't exist, R returns `NA`. `NA` is a special value in R that means "missing".


~~~{.r}
pop[100]
~~~



~~~{.output}
[1] NA

~~~




#### Subsetting data.frames

Vectors are one-dimensional objects. data.frames are two-dimensional objects.

We can subset a 2-D object by providing an index for each dimension. Rows come first, then columns, separated by a comma. E.g. this returns the 3rd entry in the 2nd column of `continents`:


~~~{.r}
continents[3, 2]
~~~



~~~{.output}
[1] 13720000

~~~

Leaving a dimension empty returns all the values for that dimension. So if we want all of the columns for the first row, we use:


~~~{.r}
continents[1, ]
~~~



~~~{.output}
  continent area_km2 population percent_total_pop pop_density
1    Africa 30370000 1022234000                15    33.65933

~~~

And if we want all the rows for the 2nd and 4th columns:


~~~{.r}
continents[, c(2, 4)]
~~~



~~~{.output}
  area_km2 percent_total_pop
1 30370000              15.0
2 42330000              14.0
3 13720000               0.0
4 43820000              60.0
5 10180000              11.0
6  9008500               0.4

~~~


> #### Challenge -- subsetting two ways {.challenge}
>
> Can you think of another way to extraact the entry in the third row and second column of `continents`?
>
> Hint: 
> - We recently saw how to extract a vector (column) from a data.frame. 
> - What vector does the value you want reside in? 
> - Once you have that vector, how can you extract a single value?

### Boolean type and subsetting 

In `continents` we saw two of the three most common data types: characters and numeric. You saw the third when making logical comparisons like `1 > 0`: Logical data. Logical data can only be TRUE or FALSE (or `NA` for missing). We can make vectorized logical comparisons too. Let's find the sparsely-populated continents, those with fewer than ten people per square kilometer:


~~~{.r}
continents$pop_density < 10
~~~



~~~{.output}
[1] FALSE FALSE  TRUE FALSE FALSE  TRUE

~~~

That went over each element in `pop_density` and compared it with 10. We say that R "recycled" 10 to compare it with each element in `pop_density`. 

In the same way that we subset by index before, we can subset by a logical vector, and we can do this in one or more dimensional subsetting.


~~~{.r}
continents$continent[continents$pop_density < 10]
~~~



~~~{.output}
[1] "Antarctica" "Oceania"   

~~~



~~~{.r}
continents[continents$pop_density < 10, ]
~~~



~~~{.output}
   continent area_km2 population percent_total_pop  pop_density
3 Antarctica 13720000       4490               0.0 0.0003272595
6    Oceania  9008500   29127000               0.4 3.2332796803

~~~

#### Two ways to subset

To be really clear, there are two similar ways to subset in R. Both use square-brackets. In one, you provide the indices of the elements you want:


~~~{.r}
continents$continent[c(2, 4, 6)]
~~~



~~~{.output}
[1] "Americas" "Asia"     "Oceania" 

~~~

In the other, you provide TRUE or FALSE for each element, TRUE if you want it, FALSE if you don't.


~~~{.r}
continents$continent[c(FALSE, TRUE, FALSE, TRUE, FALSE, TRUE)]
~~~



~~~{.output}
[1] "Americas" "Asia"     "Oceania" 

~~~



> #### MCQ -- Subset and vectorize {.challenge}
>
> What is the total land area of continents that have at least 10% of the world's population? 
>
> - Use subsetting to get the areas you want
> - Use the `sum` function to find the total land area
>
> a. 149428500
> b. 126700000
> c. 22728500
> d. 100
>

### Checking data.type

You can ask R what kind of data is assigned to a variable name with the `class` function:


~~~{.r}
class(continents)
~~~



~~~{.output}
[1] "data.frame"

~~~



~~~{.r}
class(continents$continent)
~~~



~~~{.output}
[1] "character"

~~~



~~~{.r}
class(continents$population[1])
~~~



~~~{.output}
[1] "numeric"

~~~

You can also ask specifically whether data is a particular type:


~~~{.r}
is.numeric(3)
~~~



~~~{.output}
[1] TRUE

~~~



~~~{.r}
is.numeric("three")
~~~



~~~{.output}
[1] FALSE

~~~


### Reading csv data

The `continents` data.frame was useful for learning because it was so small, but it's time to graduate to something more interesting and realistic. Data come in many forms, and we need to be able to load them in R. For our own use and with others who use R, there are R-specific data structures we can use, like the .RDA file-type we just saw, but we need to be able to work with more general data types too. Comma-separated value (csv) tables are perhaps the most universal data structure. 

The gapminder dataset provides country-by-year level data on gdp, population, and longevity. I downloaded it and put it in the data directory of my project. You will do the same in a minute.

We can read csv's with the `read.csv` function. The argument to `read.csv` is the location of the file, relative to your working directory. Since I saved the gapminder data to the `data` directory of my project, I can load it with this. Note the use of tab-completion to find the file and get it exactly right without typos. 


~~~{.r}
read.csv('data/gapminder-FiveYearData.csv')
~~~

Whoa! What just happened? R executed the function and printed the result, just like when you enter `log(1)`. How do you store an object to a variable?


~~~{.r}
gapminder <- read.csv('data/gapminder-FiveYearData.csv')
~~~

Now, a data.frame called `gapminder` is in my Environment, and I can see that it is a data.frame with 1704 rows and 6 columns. 

> #### Challenge -- read csv data {.challenge}
>
> The gapminder data are available at [this link](https://raw.githubusercontent.com/resbaz/r-novice-gapminder-files/master/data/gapminder-FiveYearData.csv). 
> - Right click on the link to "save file as..."
> - Save the .csv file in the `/data` directory of your project.
> - Read the data with the `read.csv` function and assign it to the variable `gapminder`.
> - Inspect the data.frame using the `summary` function. What is the most recent year for which we have data? 
> 
> **Advanced challenge**
>
> Suppose you get a .csv file from a colleague in Europe. Because they use "," (comma) as a decimal separator, they use ";" (semi-colons) to separate fields. How can you read it into R? 
>
> Feel free to use the web and/or R's helpfiles.    
>

