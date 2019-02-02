# Swirl-Getting-and-Cleaning-data

## Manipulating Data with dplyr

| Attempting to load lesson dependencies...

| Package ‘dplyr’ loaded correctly!

  |                                                                                           |   0%

| In this lesson, you'll learn how to manipulate data using dplyr. dplyr is a fast and powerful R
| package written by Hadley Wickham and Romain Francois that provides a consistent and concise
| grammar for manipulating tabular data.

...

  |==                                                                                         |   2%

| One unique aspect of dplyr is that the same set of tools allow you to work with tabular data from
| a variety of sources, including data frames, data tables, databases and multidimensional arrays.
| In this lesson, we'll focus on data frames, but everything you learn will apply equally to other
| formats.

...

  |===                                                                                        |   3%

| As you may know, "CRAN is a network of ftp and web servers around the world that store identical,
| up-to-date, versions of code and documentation for R" (http://cran.rstudio.com/). RStudio
| maintains one of these so-called 'CRAN mirrors' and they generously make their download logs
| publicly available (http://cran-logs.rstudio.com/). We'll be working with the log from July 8,
| 2014, which contains information on roughly 225,000 package downloads.

...

  |=====                                                                                      |   5%

| I've created a variable called path2csv, which contains the full file path to the dataset. Call
| read.csv() with two arguments, path2csv and stringsAsFactors = FALSE, and save the result in a new
| variable called mydf. Check ?read.csv if you need help.

> mydf = read.csv("path2csv", stringsAsFactors = FALSE)
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file 'path2csv': No such file or directory
> mydf = read.csv(path2csv, stringsAsFactors = FALSE)

| Try again. Getting it right on the first try is boring anyway! Or, type info() for more options.

| Store the result of read.csv(path2csv, stringsAsFactors = FALSE) in a new variable called mydf.

> mydf <- read.csv(path2csv, stringsAsFactors = FALSE)

| You got it!

  |======                                                                                     |   7%

| Use dim() to look at the dimensions of mydf.

> dim(mydf)
[1] 225468     11

| Nice work!

  |========                                                                                   |   8%

| Now use head() to preview the data.

> head(mydf)
  X       date     time   size r_version r_arch      r_os      package version country ip_id
1 1 2014-07-08 00:54:41  80589     3.1.0 x86_64   mingw32    htmltools   0.2.4      US     1
2 2 2014-07-08 00:59:53 321767     3.1.0 x86_64   mingw32      tseries 0.10-32      US     2
3 3 2014-07-08 00:47:13 748063     3.1.0 x86_64 linux-gnu        party  1.0-15      US     3
4 4 2014-07-08 00:48:05 606104     3.1.0 x86_64 linux-gnu        Hmisc  3.14-4      US     3
5 5 2014-07-08 00:46:50  79825     3.0.2 x86_64 linux-gnu       digest   0.6.4      CA     4
6 6 2014-07-08 00:48:04  77681     3.1.0 x86_64 linux-gnu randomForest   4.6-7      US     3

| All that practice is paying off!

  |=========                                                                                  |  10%

| The dplyr package was automatically installed (if necessary) and loaded at the beginning of this
| lesson. Normally, this is something you would have to do on your own. Just to build the habit,
| type library(dplyr) now to load the package again.

> library(dplyr)

| Excellent job!

  |===========                                                                                |  12%

| It's important that you have dplyr version 0.4.0 or later. To confirm this, type
| packageVersion("dplyr").

> packageVersion("dplyr")
[1] ‘0.7.4’

| All that practice is paying off!

  |============                                                                               |  13%

| If your dplyr version is not at least 0.4.0, then you should hit the Esc key now, reinstall dplyr,
| then resume this lesson where you left off.

...

  |==============                                                                             |  15%

| The first step of working with data in dplyr is to load the data into what the package authors
| call a 'data frame tbl' or 'tbl_df'. Use the following code to create a new tbl_df called cran:
| 
| cran <- tbl_df(mydf).

> cran <- tbl_df(mydf)

| Your dedication is inspiring!

  |===============                                                                            |  17%

| To avoid confusion and keep things running smoothly, let's remove the original data frame from
| your workspace with rm("mydf").

> rm("mydf")

| All that practice is paying off!

  |=================                                                                          |  18%

| From ?tbl_df, "The main advantage to using a tbl_df over a regular data frame is the printing."
| Let's see what is meant by this. Type cran to print our tbl_df to the console.

> cran
# A tibble: 225,468 x 11
       X       date     time    size r_version r_arch      r_os      package version country ip_id
   <int>      <chr>    <chr>   <int>     <chr>  <chr>     <chr>        <chr>   <chr>   <chr> <int>
 1     1 2014-07-08 00:54:41   80589     3.1.0 x86_64   mingw32    htmltools   0.2.4      US     1
 2     2 2014-07-08 00:59:53  321767     3.1.0 x86_64   mingw32      tseries 0.10-32      US     2
 3     3 2014-07-08 00:47:13  748063     3.1.0 x86_64 linux-gnu        party  1.0-15      US     3
 4     4 2014-07-08 00:48:05  606104     3.1.0 x86_64 linux-gnu        Hmisc  3.14-4      US     3
 5     5 2014-07-08 00:46:50   79825     3.0.2 x86_64 linux-gnu       digest   0.6.4      CA     4
 6     6 2014-07-08 00:48:04   77681     3.1.0 x86_64 linux-gnu randomForest   4.6-7      US     3
 7     7 2014-07-08 00:48:35  393754     3.1.0 x86_64 linux-gnu         plyr   1.8.1      US     3
 8     8 2014-07-08 00:47:30   28216     3.0.2 x86_64 linux-gnu      whisker   0.3-2      US     5
 9     9 2014-07-08 00:54:58    5928      <NA>   <NA>      <NA>         Rcpp  0.10.4      CN     6
10    10 2014-07-08 00:15:35 2206029     3.0.2 x86_64 linux-gnu     hflights     0.1      US     7
# ... with 225,458 more rows

| Nice work!

  |==================                                                                         |  20%

| This output is much more informative and compact than what we would get if we printed the original
| data frame (mydf) to the console.

...

  |====================                                                                       |  22%

| First, we are shown the class and dimensions of the dataset. Just below that, we get a preview of
| the data. Instead of attempting to print the entire dataset, dplyr just shows us the first 10 rows
| of data and only as many columns as fit neatly in our console. At the bottom, we see the names and
| classes for any variables that didn't fit on our screen.

...

  |=====================                                                                      |  23%

| According to the "Introduction to dplyr" vignette written by the package authors, "The dplyr
| philosophy is to have small functions that each do one thing well." Specifically, dplyr supplies
| five 'verbs' that cover most fundamental data manipulation tasks: select(), filter(), arrange(),
| mutate(), and summarize().

...

  |=======================                                                                    |  25%

| Use ?select to pull up the documentation for the first of these core functions.

> ?select

| That's a job well done!

  |========================                                                                   |  27%

| Help files for the other functions are accessible in the same way.

...

  |==========================                                                                 |  28%

| As may often be the case, particularly with larger datasets, we are only interested in some of the
| variables. Use select(cran, ip_id, package, country) to select only the ip_id, package, and
| country variables from the cran dataset.

> select(cran, ip_id, package, country)
# A tibble: 225,468 x 3
   ip_id      package country
   <int>        <chr>   <chr>
 1     1    htmltools      US
 2     2      tseries      US
 3     3        party      US
 4     3        Hmisc      US
 5     4       digest      CA
 6     3 randomForest      US
 7     3         plyr      US
 8     5      whisker      US
 9     6         Rcpp      CN
10     7     hflights      US
# ... with 225,458 more rows

| Your dedication is inspiring!

  |===========================                                                                |  30%

| The first thing to notice is that we don't have to type cran$ip_id, cran$package, and
| cran$country, as we normally would when referring to columns of a data frame. The select()
| function knows we are referring to columns of the cran dataset.

...

  |=============================                                                              |  32%

| Also, note that the columns are returned to us in the order we specified, even though ip_id is the
| rightmost column in the original dataset.

...

  |==============================                                                             |  33%

| Recall that in R, the `:` operator provides a compact notation for creating a sequence of numbers.
| For example, try 5:20.

> 5:20
 [1]  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

| That's a job well done!

  |================================                                                           |  35%

| Normally, this notation is reserved for numbers, but select() allows you to specify a sequence of
| columns this way, which can save a bunch of typing. Use select(cran, r_arch:country) to select all
| columns starting from r_arch and ending with country.

> select(cran, r_arch:country)
# A tibble: 225,468 x 5
   r_arch      r_os      package version country
    <chr>     <chr>        <chr>   <chr>   <chr>
 1 x86_64   mingw32    htmltools   0.2.4      US
 2 x86_64   mingw32      tseries 0.10-32      US
 3 x86_64 linux-gnu        party  1.0-15      US
 4 x86_64 linux-gnu        Hmisc  3.14-4      US
 5 x86_64 linux-gnu       digest   0.6.4      CA
 6 x86_64 linux-gnu randomForest   4.6-7      US
 7 x86_64 linux-gnu         plyr   1.8.1      US
 8 x86_64 linux-gnu      whisker   0.3-2      US
 9   <NA>      <NA>         Rcpp  0.10.4      CN
10 x86_64 linux-gnu     hflights     0.1      US
# ... with 225,458 more rows

| You are quite good my friend!

  |=================================                                                          |  37%

| We can also select the same columns in reverse order. Give it a try.

> select(cran, desc(r_arch:country))
Error in desc(r_arch:country) : object 'r_arch' not found
> select(cran, country:r_arch)
# A tibble: 225,468 x 5
   country version      package      r_os r_arch
     <chr>   <chr>        <chr>     <chr>  <chr>
 1      US   0.2.4    htmltools   mingw32 x86_64
 2      US 0.10-32      tseries   mingw32 x86_64
 3      US  1.0-15        party linux-gnu x86_64
 4      US  3.14-4        Hmisc linux-gnu x86_64
 5      CA   0.6.4       digest linux-gnu x86_64
 6      US   4.6-7 randomForest linux-gnu x86_64
 7      US   1.8.1         plyr linux-gnu x86_64
 8      US   0.3-2      whisker linux-gnu x86_64
 9      CN  0.10.4         Rcpp      <NA>   <NA>
10      US     0.1     hflights linux-gnu x86_64
# ... with 225,458 more rows

| That's the answer I was looking for.

  |===================================                                                        |  38%

| Print the entire dataset again, just to remind yourself of what it looks like. You can do this at
| anytime during the lesson.

> cran
# A tibble: 225,468 x 11
       X       date     time    size r_version r_arch      r_os      package version country ip_id
   <int>      <chr>    <chr>   <int>     <chr>  <chr>     <chr>        <chr>   <chr>   <chr> <int>
 1     1 2014-07-08 00:54:41   80589     3.1.0 x86_64   mingw32    htmltools   0.2.4      US     1
 2     2 2014-07-08 00:59:53  321767     3.1.0 x86_64   mingw32      tseries 0.10-32      US     2
 3     3 2014-07-08 00:47:13  748063     3.1.0 x86_64 linux-gnu        party  1.0-15      US     3
 4     4 2014-07-08 00:48:05  606104     3.1.0 x86_64 linux-gnu        Hmisc  3.14-4      US     3
 5     5 2014-07-08 00:46:50   79825     3.0.2 x86_64 linux-gnu       digest   0.6.4      CA     4
 6     6 2014-07-08 00:48:04   77681     3.1.0 x86_64 linux-gnu randomForest   4.6-7      US     3
 7     7 2014-07-08 00:48:35  393754     3.1.0 x86_64 linux-gnu         plyr   1.8.1      US     3
 8     8 2014-07-08 00:47:30   28216     3.0.2 x86_64 linux-gnu      whisker   0.3-2      US     5
 9     9 2014-07-08 00:54:58    5928      <NA>   <NA>      <NA>         Rcpp  0.10.4      CN     6
10    10 2014-07-08 00:15:35 2206029     3.0.2 x86_64 linux-gnu     hflights     0.1      US     7
# ... with 225,458 more rows

| Nice work!

  |====================================                                                       |  40%

| Instead of specifying the columns we want to keep, we can also specify the columns we want to
| throw away. To see how this works, do select(cran, -time) to omit the time column.

> select(cran, -time)
# A tibble: 225,468 x 10
       X       date    size r_version r_arch      r_os      package version country ip_id
   <int>      <chr>   <int>     <chr>  <chr>     <chr>        <chr>   <chr>   <chr> <int>
 1     1 2014-07-08   80589     3.1.0 x86_64   mingw32    htmltools   0.2.4      US     1
 2     2 2014-07-08  321767     3.1.0 x86_64   mingw32      tseries 0.10-32      US     2
 3     3 2014-07-08  748063     3.1.0 x86_64 linux-gnu        party  1.0-15      US     3
 4     4 2014-07-08  606104     3.1.0 x86_64 linux-gnu        Hmisc  3.14-4      US     3
 5     5 2014-07-08   79825     3.0.2 x86_64 linux-gnu       digest   0.6.4      CA     4
 6     6 2014-07-08   77681     3.1.0 x86_64 linux-gnu randomForest   4.6-7      US     3
 7     7 2014-07-08  393754     3.1.0 x86_64 linux-gnu         plyr   1.8.1      US     3
 8     8 2014-07-08   28216     3.0.2 x86_64 linux-gnu      whisker   0.3-2      US     5
 9     9 2014-07-08    5928      <NA>   <NA>      <NA>         Rcpp  0.10.4      CN     6
10    10 2014-07-08 2206029     3.0.2 x86_64 linux-gnu     hflights     0.1      US     7
# ... with 225,458 more rows

| That's a job well done!

  |======================================                                                     |  42%

| The negative sign in front of time tells select() that we DON'T want the time column. Now, let's
| combine strategies to omit all columns from X through size (X:size). To see how this might work,
| let's look at a numerical example with -5:20.

> -5:20
 [1] -5 -4 -3 -2 -1  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

| That's the answer I was looking for.

  |=======================================                                                    |  43%

| Oops! That gaves us a vector of numbers from -5 through 20, which is not what we want. Instead, we
| want to negate the entire sequence of numbers from 5 through 20, so that we get -5, -6, -7, ... ,
| -18, -19, -20. Try the same thing, except surround 5:20 with parentheses so that R knows we want
| it to first come up with the sequence of numbers, then apply the negative sign to the whole thing.

> -(5:20)
 [1]  -5  -6  -7  -8  -9 -10 -11 -12 -13 -14 -15 -16 -17 -18 -19 -20

| All that practice is paying off!

  |=========================================                                                  |  45%

| Use this knowledge to omit all columns X:size using select().

> select(cran, -(X:size))
# A tibble: 225,468 x 7
   r_version r_arch      r_os      package version country ip_id
       <chr>  <chr>     <chr>        <chr>   <chr>   <chr> <int>
 1     3.1.0 x86_64   mingw32    htmltools   0.2.4      US     1
 2     3.1.0 x86_64   mingw32      tseries 0.10-32      US     2
 3     3.1.0 x86_64 linux-gnu        party  1.0-15      US     3
 4     3.1.0 x86_64 linux-gnu        Hmisc  3.14-4      US     3
 5     3.0.2 x86_64 linux-gnu       digest   0.6.4      CA     4
 6     3.1.0 x86_64 linux-gnu randomForest   4.6-7      US     3
 7     3.1.0 x86_64 linux-gnu         plyr   1.8.1      US     3
 8     3.0.2 x86_64 linux-gnu      whisker   0.3-2      US     5
 9      <NA>   <NA>      <NA>         Rcpp  0.10.4      CN     6
10     3.0.2 x86_64 linux-gnu     hflights     0.1      US     7
# ... with 225,458 more rows

| You nailed it! Good job!

  |==========================================                                                 |  47%

| Now that you know how to select a subset of columns using select(), a natural next question is
| "How do I select a subset of rows?" That's where the filter() function comes in.

...

  |============================================                                               |  48%

| Use filter(cran, package == "swirl") to select all rows for which the package variable is equal to
| "swirl". Be sure to use two equals signs side-by-side!
