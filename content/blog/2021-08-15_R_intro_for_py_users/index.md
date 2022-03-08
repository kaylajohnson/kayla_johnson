---
title: "Introduction to R and the Tidyverse for python users"
subtitle: ""
excerpt: "Get started with R quickly if you already know python"
date: 2021-08-15
author: "Kayla Johnson"
draft: false
images:
series:
tags:
  - python
  - R
categories:
  - python
  - R
layout: single
---

This introduction is a post converted from a small workshop I gave to help my python-using colleagues get started with R and the Tidyverse packages. It is meant to be an introduction to the main things you would need to know about R coming from python so that you do not need to read an introduction to programming and R. As a python user, you understand the idea of a package, you just need to know the R equivalent of `import pandas`. I try to cover the basics, the most common data tidying and wrangling tasks, and data visualization in this post.

## Quick RStudio intro
There are multiple popular text editors and IDEs amongst python users, but in R, there is really only one wildy popular IDE, which is RStudio. I suggest checking out this RStudio support [page](https://support.rstudio.com/hc/en-us/articles/200549016-Customizing-the-RStudio-IDE/) for a pretty comprehensive list of customizations to tweak the default behavior and appearance of RStudio. As of RStudio version 1.4, you can now also write python code in RStudio! Check out [Three Ways to Program in Python With RStudio](https://www.rstudio.com/blog/three-ways-to-program-in-python-with-rstudio/) to learn how to use python alone or R and python together within RStudio. 

## How R is not python basics
**Installing packages:**
You will want to use `install.packages("package_name")` to install packages from [CRAN](https://cran.r-project.org/), the Comprehensive R Archive Network. Packages from Bioconductor must be installed from Bioconductor, not CRAN. See this [page](https://www.bioconductor.org/install/) to install Bioconductor. Looking up any package in Bioconductor will also give explicit instructions. Packages can be installed from GitHub if the `devtools` package is installed from CRAN first. Individual developers will generally give those instructions in their documentation.

**Loading packages:**
In python, you `import` packages. The R equivalent is `library(package_name)`. Today we are focused on base R and the Tidyverse, so I will only be loading `tidyverse`. It is also worth noting that in python, you can `import pandas as pd` and then access pandas functions with `pd.read_csv(args)`. In R, the functions of a package become available to use without a package prefix once you load the package. So, after loading the `readr` package, you can just use `read_csv(args)` because `read_csv` is a function in the `readr` package. However, you can explicitly tell R which package the function should be pulled from, e.g. `readr::read_csv(args)`. This is helpful if you have loaded two or more packages with different functions that are named the same. When loading a package, R will tell you if there is a name conflict with other loaded packages. These should be rare, but it is worth knowing!

```r
# if you have not installed it yet: install.packages("tidyverse")

# this line is about as common in R as "import pandas as pd" is in python
# load the tidyverse
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.0.2     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

R has vectors and lists, but not dictionaries. The main difference is vectors are one data type (character, numeric, etc) and lists can mix data types, including more lists. Notice here I am using `<-` as the assignment operator instead of `=` as you would use in python. Using `=` as the assignment operator does work in R (and I use it in the next code chunk to prove it to you), but [Google's R Style Guide](http://web.stanford.edu/class/cs109l/unrestricted/resources/google-style.html) and the [Style Guide in _Advanced R_](http://adv-r.had.co.nz/Style.html) by Hadley Wickham recommend using `<-` as the assignment operator.

```r
# create a vector
a <- c(1, "a", 2.6)
print(a)
```

```
## [1] "1"   "a"   "2.6"
```

```r
print(class(a))
```

```
## [1] "character"
```

```r
# can see from output that even when we put 1 and 2.6 in as numbers, 
# they are converted to strings because "a" is a string and cannot be a number
```

Indexing a vector in R is similar to indexing lists in python but not quite the same. In the first line I am using one of the constants built into R (LETTERS), which contains the 26 upper-case letters of the Roman alphabet. See the few built-in constants [here](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Constants.html). R is 1-indexed instead of 0-indexed and using `a[3:6]` is inclusive of the last element (6th element in this case). Also, using `-` removes elements, it does not select from the end as python does.

```r
# proof that using = as an assignment operator works
a = LETTERS[1:10]

# first element
a[1]
```

```
## [1] "A"
```

```r
# elements 3 through 6 - 3,4,5 and 6
a[3:6]
```

```
## [1] "C" "D" "E" "F"
```

```r
# gets rid of first element, does NOT select from the end
a[-1]
```

```
## [1] "B" "C" "D" "E" "F" "G" "H" "I" "J"
```

Lists are somewhat similar to python dictionaries in that they can have "names" that are a bit like keys. Here I create `my_list` and give names with `names()`.

```r
# create a list
my_list <- list(c(1,2,3), c(4,5,6), c(7,8,9))
my_list
```

```
## [[1]]
## [1] 1 2 3
## 
## [[2]]
## [1] 4 5 6
## 
## [[3]]
## [1] 7 8 9
```

```r
# add names to that list
names(my_list) <- c("a", "b", "c")
my_list
```

```
## $a
## [1] 1 2 3
## 
## $b
## [1] 4 5 6
## 
## $c
## [1] 7 8 9
```

You can also designate the names while creating the list. 

```r
# if you know the names from the start this is equivalent
my_list <- list(a = c(1,2,3), b = c(4,5,6), c = c(7,8,9))
```

You can access lists with `$` or brackets, but `$` only works on named lists, it will not work with positions e.g. `my_list$1`.

```r
# this gets the values of the list named "a" in my_list
my_list$a
```

```
## [1] 1 2 3
```

When you use brackets to access lists you have the option of using single brackets or double brackets. It might be useful to think of double brackets as being similar to using `.values` in python. Single brackets get the entire object, including the name of the list, while double brackets gets you just the elements of the list. The `$` operator works like the double brackets

```r
# single brackets gets the whole list object
my_list["a"]
```

```
## $a
## [1] 1 2 3
```

```r
# double brackets gets the values of the list
my_list[["a"]]
```

```
## [1] 1 2 3
```

```r
# can also use the position instead of the name
my_list[1]
```

```
## $a
## [1] 1 2 3
```

```r
my_list[[1]]
```

```
## [1] 1 2 3
```

Vector math exists in R without importing packages. 

```r
c(1,2,3) * c(4,5,6)
```

```
## [1]  4 10 18
```

```r
c(1,2,3) * 2
```

```
## [1] 2 4 6
```

R dataframes are similar to Pandas dataframes. Since we are working mostly with the tidyverse, I also need to mention tibbles. Tibbles and dataframes are pretty similar in practice, but tibbles have a few nicer features. The features are nice enough that I would say always use a tibble when explicitly creating something, but you don't need to be in constant panic about whether something is a dataframe or tibble (although if you need to check, use `class()`). Tidyverse functions output tibbles. You can also convert a dataframe to a tibble with `as_tibble()`. Almost any function that works on a dataframe will work on a tibble.

```r
# creating a tibble
tbl <- tibble(letter = LETTERS, 
              number = c(1:26), 
              group = c(rep("first", 8), 
                        rep("second", 10), rep("third", 8)))
tbl
```

```
## # A tibble: 26 × 3
##    letter number group 
##    <chr>   <int> <chr> 
##  1 A           1 first 
##  2 B           2 first 
##  3 C           3 first 
##  4 D           4 first 
##  5 E           5 first 
##  6 F           6 first 
##  7 G           7 first 
##  8 H           8 first 
##  9 I           9 second
## 10 J          10 second
## # … with 16 more rows
```

```r
# creating a dataframe would look the same, except you use the data.frame() function instead of tibble()
```

Indexing dataframes and tibbles works the same way, but it is slightly different from Pandas dataframes. The concepts of loc and iloc are not a thing.

```r
# choose particular value (in this case, 2nd row, 3rd column)
tbl[2,3]
```

```
## # A tibble: 1 × 1
##   group
##   <chr>
## 1 first
```


```r
# choose whole column by position (first column in this case)
tbl[,1]
```

```
## # A tibble: 26 × 1
##    letter
##    <chr> 
##  1 A     
##  2 B     
##  3 C     
##  4 D     
##  5 E     
##  6 F     
##  7 G     
##  8 H     
##  9 I     
## 10 J     
## # … with 16 more rows
```


```r
# choose whole column by name
tbl[,"group"]
```

```
## # A tibble: 26 × 1
##    group 
##    <chr> 
##  1 first 
##  2 first 
##  3 first 
##  4 first 
##  5 first 
##  6 first 
##  7 first 
##  8 first 
##  9 second
## 10 second
## # … with 16 more rows
```


```r
# choose all column values with $
# this only works with column names, not column positions
tbl$letter
```

```
##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J" "K" "L" "M" "N" "O" "P" "Q" "R" "S"
## [20] "T" "U" "V" "W" "X" "Y" "Z"
```


```r
# tidyverse also includes a handy pull function that chooses 
# the values of a given column, not the whole column itself. 
# This is especially useful because it can be piped into 
# (we'll see pipes later)
pull(tbl, 1)
```

```
##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J" "K" "L" "M" "N" "O" "P" "Q" "R" "S"
## [20] "T" "U" "V" "W" "X" "Y" "Z"
```


```r
# get whole (second) row
tbl[2,]
```

```
## # A tibble: 1 × 3
##   letter number group
##   <chr>   <int> <chr>
## 1 B           2 first
```

You can set column names with `colnames()` or change them from what they currently are.

```r
# changing colnames 
colnames(tbl) <- c("Letter", "Number", "Group")
tbl
```

```
## # A tibble: 26 × 3
##    Letter Number Group 
##    <chr>   <int> <chr> 
##  1 A           1 first 
##  2 B           2 first 
##  3 C           3 first 
##  4 D           4 first 
##  5 E           5 first 
##  6 F           6 first 
##  7 G           7 first 
##  8 H           8 first 
##  9 I           9 second
## 10 J          10 second
## # … with 16 more rows
```

The most equivalent thing to a numpy array is a matrix. These are part of base R, no package necessary. A matrix can only contain one data type and you can set row and/or column names. Selecting works similar to dataframes, but you cannot use the `$` operator. 

```r
my_matrix <- matrix(data = c(1:9), nrow = 3, ncol = 3,
                    byrow = FALSE)

rownames(my_matrix) <- c("first", "second", "third")
colnames(my_matrix) <- c("one", "two", "three")
my_matrix
```

```
##        one two three
## first    1   4     7
## second   2   5     8
## third    3   6     9
```

If and if... else statements are formatted differently in R, since nothing in R is dependent on indentation.

```r
num <- 3
if (num < 10){
  print("The number is less than 10")
}
```

```
## [1] "The number is less than 10"
```


```r
num <- 15
if (num < 10){
  print("The number is less than 10")
} else {
  print("The number is NOT less than 10")
}
```

```
## [1] "The number is NOT less than 10"
```


```r
num <- 15
if (num < 10){
  print("The number is less than 10")
} else if (num == 10) {
  print("The number is 10")
} else {
  print("The number is greater than 10")
}
```

```
## [1] "The number is greater than 10"
```

Here I am creating a custom function that converts degrees Fahrenheit to degrees Celsius to show how custom functions are written. 

```r
FtoC <- function(F){
  C <- (F - 32) * (5/9)
  return(C)
}

FtoC(50)
```

```
## [1] 10
```

## Tidyverse

### Reading/writing (readr)

#### read_delim
This is the general format for how you read in data from a file. This is also a good place to point out that you need to use `T` or `TRUE` and `F` or `FALSE` instead of `True` and `False`. 

```r
# args
# file = path to file to read in
# delim = the file delimiter (== 'sep' in pandas)
# col_names is T/F for whether the columns have names
# col_types explicitly gives column types for the file
# here col_types is saying read the columns in as "double",
# except the column named "gene", which is a "character" column
data <- read_delim(file = "some_tsv_file_path", delim = "\t", 
                   col_names = F, col_types = cols(.default = "d", gene = "c"))
```

The `col_types` argument is not necessary, but can be helpful to include. By default (can be changed with `guess_max` argument), `read_delim()` guesses the column types from the first 1000 rows. Usually this is more than enough but sometimes causes problems for weird variables in huge files, so thought I'd point it out. There is also a `read_tsv()` and `read_csv()`, which bypass the need for a delim argument. 

#### write_delim
Write data:

```r
 data %>% write_delim("path_to_save_to", delim = "\t", col_names = T)
```
This is the first time we have seen the pipe operator ( `%>%` ). This is from the `magrittr` package, which is part of the tidyverse (and therefore loaded when you call `library(tidyverse)`). It pipes whatever is to the left of it (in this case `data`, which is a dataframe/tibble) into the function to the right of it as the first argument (in this case, `write_delim()`). Tidyverse functions are set up so that data is always the first argument, so pipes can be "stacked" e.g. `data %>% function_one(some_args) %>% function_two(some_args, more_args)`, but this also means you can use it with non-tidyverse functions, e.g. `c(1,2,3) %>% mean()`. I am not recommending that use of of the pipe, but it does work.

### Data transformation (dplyr)
This section contains the main `dplyr` functions, which help you transform data. The tidyverse package has some nice built-in data sets to help practice and teach functions, so I am going to use one of those data sets, `msleep`. The `msleep` data set contains the sleep times and weights of a set of mammals, along with their genus, order, and `vore` which tells you their diet type.


```r
# look at our msleep dataset
msleep
```

```
## # A tibble: 83 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  3 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  4 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  5 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  6 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  7 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  8 Vespe… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  9 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## 10 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
## # … with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

#### Filter - for filtering by row
Here is how to filter for only the mammals in order Rodentia AND whose conservation status is "domesticated."

```r
# find mammals in the dataset that are domesticated rodents 
d_rodents <- filter(msleep, order == "Rodentia", conservation == "domesticated")
# equivalent
d_rodents <- msleep %>% filter(order == "Rodentia" & conservation == "domesticated")
# equivalent
d_rodents <- msleep %>% 
  filter(order == "Rodentia") %>% 
  filter(conservation == "domesticated")
 
d_rodents
```

```
## # A tibble: 2 × 11
##   name   genus  vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>  <chr>  <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Guine… Cavis  herbi Rode… domesticated         9.4       0.8       0.217  14.6
## 2 Chinc… Chinc… herbi Rode… domesticated        12.5       1.5       0.117  11.5
## # … with 2 more variables: brainwt <dbl>, bodywt <dbl>
```

And here is an example of OR filtering to find mammals that are herbivores or omnivores. The `|` is the or operator.

```r
# find mammals that are either herbivores or omnivores
plant_eaters <- filter(msleep, vore == "herbi" | vore == "omni")
plant_eaters
```

```
## # A tibble: 52 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  2 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  3 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  4 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  5 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  6 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
##  7 Goat   Capri herbi Arti… lc                   5.3       0.6      NA      18.7
##  8 Guine… Cavis herbi Rode… domesticated         9.4       0.8       0.217  14.6
##  9 Grivet Cerc… omni  Prim… lc                  10         0.7      NA      14  
## 10 Chinc… Chin… herbi Rode… domesticated        12.5       1.5       0.117  11.5
## # … with 42 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

Another way to filter for only herbivores/omnivores is to use `%in%`. You cannot use `vore == c("herbi", "omni")`. If you do, R will recycle the c("herbi", "omni") vector so that the first row would be kept if vore == "herbi", second row would be kept if vore == "omni", and so on and so forth. Using `%in%` ensures each row is compared to all vector elements (i.e. "herbi" AND "omni"). Vector recycling is a normal part of R, see next code chunk for example.

```r
# equivalent to above
plant_eaters <- filter(msleep, vore %in% c("herbi", "omni"))
plant_eaters
```

```
## # A tibble: 52 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  2 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  3 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  4 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  5 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  6 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
##  7 Goat   Capri herbi Arti… lc                   5.3       0.6      NA      18.7
##  8 Guine… Cavis herbi Rode… domesticated         9.4       0.8       0.217  14.6
##  9 Grivet Cerc… omni  Prim… lc                  10         0.7      NA      14  
## 10 Chinc… Chin… herbi Rode… domesticated        12.5       1.5       0.117  11.5
## # … with 42 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

In this example the output shows that 1 was multiplied by 1, 2 by 2, 3 by 1, and 4 by 2, because the shorter vector (`c(1,2)`) was recycled to properly do multiplication on all elements.

```r
# example for vector recycling:
c(1,2,3,4) * c(1,2)
```

```
## [1] 1 4 3 8
```

Knowing `is.na()` and that `!` means "not" is helpful.

```r
# this gets all data that is not missing conservation status
# (conservation is not NA)
has_conservation <- msleep %>% filter(!is.na(conservation))
has_conservation
```

```
## # A tibble: 54 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  3 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  4 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  5 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  6 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
##  7 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
##  8 Goat   Capri herbi Arti… lc                   5.3       0.6      NA      18.7
##  9 Guine… Cavis herbi Rode… domesticated         9.4       0.8       0.217  14.6
## 10 Grivet Cerc… omni  Prim… lc                  10         0.7      NA      14  
## # … with 44 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```


#### Arrange - for ordering the rows
Arrange orders the tibble by the columns you choose in the order you put them.

```r
# order by sleep_total, then sleep_rem 
# (can order by as many as you want in the order you put them)
arrange(msleep, sleep_total, sleep_rem)
```

```
## # A tibble: 83 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Giraf… Gira… herbi Arti… cd                   1.9       0.4      NA      22.1
##  2 Pilot… Glob… carni Ceta… cd                   2.7       0.1      NA      21.4
##  3 Horse  Equus herbi Peri… domesticated         2.9       0.6       1      21.1
##  4 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
##  5 Donkey Equus herbi Peri… domesticated         3.1       0.4      NA      20.9
##  6 Afric… Loxo… herbi Prob… vu                   3.3      NA        NA      20.7
##  7 Caspi… Phoca carni Carn… vu                   3.5       0.4      NA      20.5
##  8 Sheep  Ovis  herbi Arti… domesticated         3.8       0.6      NA      20.2
##  9 Asian… Elep… herbi Prob… en                   3.9      NA        NA      20.1
## 10 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
## # … with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

By default `arrange()` orders in ascending order, use `desc()` to get descending order.

```r
# descending order
arrange(msleep, desc(sleep_total), desc(sleep_rem))
```

```
## # A tibble: 83 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Littl… Myot… inse… Chir… <NA>                19.9       2         0.2     4.1
##  2 Big b… Epte… inse… Chir… lc                  19.7       3.9       0.117   4.3
##  3 Thick… Lutr… carni Dide… lc                  19.4       6.6      NA       4.6
##  4 Giant… Prio… inse… Cing… en                  18.1       6.1      NA       5.9
##  5 North… Dide… omni  Dide… lc                  18         4.9       0.333   6  
##  6 Long-… Dasy… carni Cing… lc                  17.4       3.1       0.383   6.6
##  7 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  8 Arcti… Sper… herbi Rode… lc                  16.6      NA        NA       7.4
##  9 Golde… Sper… herbi Rode… lc                  15.9       3        NA       8.1
## 10 Tiger  Pant… carni Carn… en                  15.8      NA        NA       8.2
## # … with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

#### Select - for choosing columns

```r
# select all columns name to conservation (name, genus, vore, order)
smaller_msleep <- select(msleep, name:conservation)
# this is equivalent, since name, genus, vore, order are the first four columns
smaller_msleep <- select(msleep, 1:4)
smaller_msleep
```

```
## # A tibble: 83 × 4
##    name                       genus       vore  order       
##    <chr>                      <chr>       <chr> <chr>       
##  1 Cheetah                    Acinonyx    carni Carnivora   
##  2 Owl monkey                 Aotus       omni  Primates    
##  3 Mountain beaver            Aplodontia  herbi Rodentia    
##  4 Greater short-tailed shrew Blarina     omni  Soricomorpha
##  5 Cow                        Bos         herbi Artiodactyla
##  6 Three-toed sloth           Bradypus    herbi Pilosa      
##  7 Northern fur seal          Callorhinus carni Carnivora   
##  8 Vesper mouse               Calomys     <NA>  Rodentia    
##  9 Dog                        Canis       carni Carnivora   
## 10 Roe deer                   Capreolus   herbi Artiodactyla
## # … with 73 more rows
```


```r
# select all columns except vore
no_vore <- select(msleep, -vore)
no_vore
```

```
## # A tibble: 83 × 10
##    name       genus  order  conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>      <chr>  <chr>  <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheetah    Acino… Carni… lc                  12.1      NA        NA      11.9
##  2 Owl monkey Aotus  Prima… <NA>                17         1.8      NA       7  
##  3 Mountain … Aplod… Roden… nt                  14.4       2.4      NA       9.6
##  4 Greater s… Blari… Soric… lc                  14.9       2.3       0.133   9.1
##  5 Cow        Bos    Artio… domesticated         4         0.7       0.667  20  
##  6 Three-toe… Brady… Pilosa <NA>                14.4       2.2       0.767   9.6
##  7 Northern … Callo… Carni… vu                   8.7       1.4       0.383  15.3
##  8 Vesper mo… Calom… Roden… <NA>                 7        NA        NA      17  
##  9 Dog        Canis  Carni… domesticated        10.1       2.9       0.333  13.9
## 10 Roe deer   Capre… Artio… lc                   3        NA        NA      21  
## # … with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```


```r
# select all columns except brainwt and bodywt
no_wt <- select(msleep, -brainwt, -bodywt)
no_wt
```

```
## # A tibble: 83 × 9
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  3 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  4 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  5 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  6 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  7 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  8 Vespe… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  9 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## 10 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
## # … with 73 more rows
```


```r
# select specific columns 
choice_cols <- select(msleep, name, order, conservation)
choice_cols
```

```
## # A tibble: 83 × 3
##    name                       order        conservation
##    <chr>                      <chr>        <chr>       
##  1 Cheetah                    Carnivora    lc          
##  2 Owl monkey                 Primates     <NA>        
##  3 Mountain beaver            Rodentia     nt          
##  4 Greater short-tailed shrew Soricomorpha lc          
##  5 Cow                        Artiodactyla domesticated
##  6 Three-toed sloth           Pilosa       <NA>        
##  7 Northern fur seal          Carnivora    vu          
##  8 Vesper mouse               Rodentia     <NA>        
##  9 Dog                        Carnivora    domesticated
## 10 Roe deer                   Artiodactyla lc          
## # … with 73 more rows
```

There are a number of helper functions you can use within select():

* `starts_with("abc")`: matches names that begin with “abc”.

* `ends_with("xyz")`: matches names that end with “xyz”.

* `contains("ijk")`: matches names that contain “ijk”.

* `matches("(.)\\1")`: selects variables that match a regular expression. This one matches any variables that contain repeated characters. 

* `num_range("x", 1:3)`: matches x1, x2 and x3. A more realistic example would maybe look like num_range("wk", 10:13) to select columns called wk10, wk11, wk12, wk13

* `stringr` is a great package for working with strings

Select can also be used to reaarange columns, since they are placed in the order you choose them in. If you only care about a few columns being first, you can use `everything()` to select everything not already explicitly stated.

```r
# for whatever reason you want sleep_total to be first col, sleep_rem to be second col, 
# and don't care about the order of the rest
# this slects sleep_total first, sleep_rem second, then everything() selects everything else
select(msleep, sleep_total, sleep_rem, everything())
```

```
## # A tibble: 83 × 11
##    sleep_total sleep_rem name   genus vore  order conservation sleep_cycle awake
##          <dbl>     <dbl> <chr>  <chr> <chr> <chr> <chr>              <dbl> <dbl>
##  1        12.1      NA   Cheet… Acin… carni Carn… lc                NA      11.9
##  2        17         1.8 Owl m… Aotus omni  Prim… <NA>              NA       7  
##  3        14.4       2.4 Mount… Aplo… herbi Rode… nt                NA       9.6
##  4        14.9       2.3 Great… Blar… omni  Sori… lc                 0.133   9.1
##  5         4         0.7 Cow    Bos   herbi Arti… domesticated       0.667  20  
##  6        14.4       2.2 Three… Brad… herbi Pilo… <NA>               0.767   9.6
##  7         8.7       1.4 North… Call… carni Carn… vu                 0.383  15.3
##  8         7        NA   Vespe… Calo… <NA>  Rode… <NA>              NA      17  
##  9        10.1       2.9 Dog    Canis carni Carn… domesticated       0.333  13.9
## 10         3        NA   Roe d… Capr… herbi Arti… lc                NA      21  
## # … with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

#### Rename - for changing column names

```r
# rename the "sleep_total" column "total_sleep"
msleep <- rename(msleep, total_sleep = sleep_total)
msleep
```

```
## # A tibble: 83 × 11
##    name   genus vore  order conservation total_sleep sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  3 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  4 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  5 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  6 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  7 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  8 Vespe… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  9 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## 10 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
## # … with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```


```r
# change back so I don't forget 
msleep <- rename(msleep, sleep_total = total_sleep)
```

#### mutate - for adding new variables
Mutate just adds each column you create to the end of the tibble. If you only want to keep the cols you are creating in a new variable, use `transmute()`.

```r
# make new col percent_rem = rem percent of total sleep
# make new col brainwt_percent = brain percent of body wt
mutate(msleep,
  percent_rem = sleep_rem / sleep_total, 
  brainwt_percent = brainwt / bodywt) 
```

```
## # A tibble: 83 × 13
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  3 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  4 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  5 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  6 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  7 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  8 Vespe… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  9 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## 10 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
## # … with 73 more rows, and 4 more variables: brainwt <dbl>, bodywt <dbl>,
## #   percent_rem <dbl>, brainwt_percent <dbl>
```

#### group_by + summarise - for summarizing data by group
Summarise can be used on its own, say if you wanted the mean sleep time across all mammals in the data set.

```r
summarise(msleep, mean_sleep = mean(sleep_total, na.rm = TRUE))
```

```
## # A tibble: 1 × 1
##   mean_sleep
##        <dbl>
## 1       10.4
```

Generally though, summarise is more useful in conjunction with `group_by()`. This shouldn't be unfamiliar from using `.groupby` in pandas. You can also group by multiple columns

```r
# get average sleep in each order
msleep %>% 
group_by(order) %>% 
summarise(mean_sleep = mean(sleep_total, na.rm = TRUE))
```

```
## # A tibble: 19 × 2
##    order           mean_sleep
##    <chr>                <dbl>
##  1 Afrosoricida         15.6 
##  2 Artiodactyla          4.52
##  3 Carnivora            10.1 
##  4 Cetacea               4.5 
##  5 Chiroptera           19.8 
##  6 Cingulata            17.8 
##  7 Didelphimorphia      18.7 
##  8 Diprotodontia        12.4 
##  9 Erinaceomorpha       10.2 
## 10 Hyracoidea            5.67
## 11 Lagomorpha            8.4 
## 12 Monotremata           8.6 
## 13 Perissodactyla        3.47
## 14 Pilosa               14.4 
## 15 Primates             10.5 
## 16 Proboscidea           3.6 
## 17 Rodentia             12.5 
## 18 Scandentia            8.9 
## 19 Soricomorpha         11.1
```

Counting is something I seem to do a lot, so here is how `summarise()` can be used for that.

```r
# find how many mammals are in each order
msleep %>% 
  group_by(order) %>% 
  summarise(total = n())
```

```
## # A tibble: 19 × 2
##    order           total
##    <chr>           <int>
##  1 Afrosoricida        1
##  2 Artiodactyla        6
##  3 Carnivora          12
##  4 Cetacea             3
##  5 Chiroptera          2
##  6 Cingulata           2
##  7 Didelphimorphia     2
##  8 Diprotodontia       2
##  9 Erinaceomorpha      2
## 10 Hyracoidea          3
## 11 Lagomorpha          1
## 12 Monotremata         1
## 13 Perissodactyla      3
## 14 Pilosa              1
## 15 Primates           12
## 16 Proboscidea         2
## 17 Rodentia           22
## 18 Scandentia          1
## 19 Soricomorpha        5
```

You can use summarize to create more than one new variable at once, as long as you're summarizing the same groups of course.

```r
# find shortest and longest sleeping mammal from each order
msleep %>% 
  group_by(order) %>% 
  summarise(shortest = min(sleep_total, na.rm = T), 
            longest = max(sleep_total, na.rm = T))
```

```
## # A tibble: 19 × 3
##    order           shortest longest
##    <chr>              <dbl>   <dbl>
##  1 Afrosoricida        15.6    15.6
##  2 Artiodactyla         1.9     9.1
##  3 Carnivora            3.5    15.8
##  4 Cetacea              2.7     5.6
##  5 Chiroptera          19.7    19.9
##  6 Cingulata           17.4    18.1
##  7 Didelphimorphia     18      19.4
##  8 Diprotodontia       11.1    13.7
##  9 Erinaceomorpha      10.1    10.3
## 10 Hyracoidea           5.3     6.3
## 11 Lagomorpha           8.4     8.4
## 12 Monotremata          8.6     8.6
## 13 Perissodactyla       2.9     4.4
## 14 Pilosa              14.4    14.4
## 15 Primates             8      17  
## 16 Proboscidea          3.3     3.9
## 17 Rodentia             7      16.6
## 18 Scandentia           8.9     8.9
## 19 Soricomorpha         8.4    14.9
```

### Joining dataframes
Sometimes you need to combine one or more dataframes. In this section, x and y are dataframes

Simple combination functions:

* `bind_rows(x, y)` will stack dfs x and y on top of each other if they have same cols

* `bind_cols(x, y)` will put x and y together if they have same rows 

Joins need a "by" argument to tell them which columns to join by (to match up the observations to combine the dfs). 

Mutating joins:

* `inner_join(x, y, by = c("some_col", "another_col"))` (keeps only observations that are present in both x and y)

* `left_join(x, y, by = "some_col")`  (keeps only observations that are present in x)

* `right_join(x, y, by = "some_col")` (keeps only observations that are present in y)

* `full_join(x, y, by = "some_col")`  (keeps only observations that are present in either x or y)

For visual explanation of joins, see R for Data Science, [here](https://r4ds.had.co.nz/relational-data.html#understanding-joins). These should be somewhat familiar from pandas.

Filtering joins:

* `semi_join(x, y)` keeps all observations in x that have a match in y.

* `anti_join(x, y)` drops all observations in x that have a match in y.

To be honest the only join I use regularly is `left_join()`.

### Tidy data (tidyr)

The functions in tidyr are helpful for making untidy data tidy. See the "Tidy Data" section in _R for Data Science_ [here](https://r4ds.had.co.nz/tidy-data.html#tidy-data-1) to learn about the principles of tidy data. Here I am going to make a couple small tibbles so that we can see what the functions are doing. The data in these tibbles will contain the height and weight of 3 (fictional) growing dogs in 2020 and 2021 but each will be untidy in its own way to demonstrate the use of `tidyr` functions. 

The `value` column of `dog_tbl` contains the values for both height and weight, which is not tidy.

```r
# untidy in its own special way
dog_tbl <- tibble(dog = c(rep("Minnie", 4), rep("Capone", 4), rep("Nayla", 4)),
                  year = rep(c(2020, 2020, 2021, 2021), 3),
                  value_type = rep(c("height", "weight"), 6),
                  value =  c(8, 7, 12, 12,
                             22, 60, 24, 75,
                             18, 32, 21, 40))

dog_tbl
```

```
## # A tibble: 12 × 4
##    dog     year value_type value
##    <chr>  <dbl> <chr>      <dbl>
##  1 Minnie  2020 height         8
##  2 Minnie  2020 weight         7
##  3 Minnie  2021 height        12
##  4 Minnie  2021 weight        12
##  5 Capone  2020 height        22
##  6 Capone  2020 weight        60
##  7 Capone  2021 height        24
##  8 Capone  2021 weight        75
##  9 Nayla   2020 height        18
## 10 Nayla   2020 weight        32
## 11 Nayla   2021 height        21
## 12 Nayla   2021 weight        40
```

The `hw_ratio` column contains the values for both height and weight in the form of a height to weight ratio, which is not convenient for working with one or the other.

```r
# untidy in its own special way
dog_tbl2 <- tibble(dog = c("Minnie", "Minnie", "Capone", "Capone", "Nayla", "Nayla"),
                   year = rep(c(2020, 2021), 3),
                   hw_ratio = c("8/7", "12/12", "22/60", "24/75", "18/32", "21/40"))


dog_tbl2
```

```
## # A tibble: 6 × 3
##   dog     year hw_ratio
##   <chr>  <dbl> <chr>   
## 1 Minnie  2020 8/7     
## 2 Minnie  2021 12/12   
## 3 Capone  2020 22/60   
## 4 Capone  2021 24/75   
## 5 Nayla   2020 18/32   
## 6 Nayla   2021 21/40
```

In this case, we have two tibbles, A_dog_tbl and B_dog_tbl, where A_dog_tbl contains the height values and B_dog_tbl contains the weight values. Each year has their own column. Fun fact demonstrated in this code chunk, you need go use back ticks to create or access column names that are completely numeric or contain symbols.

```r
# height
A_dog_tbl <- tibble(dog = c("Minnie", "Capone", "Nayla"),
                    `2020` = c(8, 22, 18), # back ticks
                    `2021` = c(12, 24, 21))

# weight
B_dog_tbl <- tibble(dog = c("Minnie", "Capone", "Nayla"),
                    `2020` = c(7, 60, 32), 
                    `2021` = c(12, 75, 40))
```


```r
A_dog_tbl
```

```
## # A tibble: 3 × 3
##   dog    `2020` `2021`
##   <chr>   <dbl>  <dbl>
## 1 Minnie      8     12
## 2 Capone     22     24
## 3 Nayla      18     21
```


```r
B_dog_tbl
```

```
## # A tibble: 3 × 3
##   dog    `2020` `2021`
##   <chr>   <dbl>  <dbl>
## 1 Minnie      7     12
## 2 Capone     60     75
## 3 Nayla      32     40
```

#### pivot_longer
This function helps us tidy datasets that uses values of a variable as column names. In the case of `A_dog_tbl`, we would like to turn the columns `2020` and `2021` into one column called `year` and one column called `height`. In other words, we are going to _pivot_ the offending columns into a new pair of variables. This increases the number of rows, thus the name "pivot_longer" - we're making the tibble longer.

```r
# another look at A_dog_tbl
A_dog_tbl
```

```
## # A tibble: 3 × 3
##   dog    `2020` `2021`
##   <chr>   <dbl>  <dbl>
## 1 Minnie      8     12
## 2 Capone     22     24
## 3 Nayla      18     21
```


```r
# a tidier version of A_dog_tbl
# we supply the columns that need to be "pivoted": c(`2020`, `2021`)
# the names_to arg places the names of the original columns (`2020`, `2021`) 
# in a new variable we call "year"
# the values_to arg places the values of the original columns (`2020`, `2021`) in
# a new variable we call "height"
A_dog_tbl %>% 
  pivot_longer(c(`2020`, `2021`), names_to = "year", values_to = "height")
```

```
## # A tibble: 6 × 3
##   dog    year  height
##   <chr>  <chr>  <dbl>
## 1 Minnie 2020       8
## 2 Minnie 2021      12
## 3 Capone 2020      22
## 4 Capone 2021      24
## 5 Nayla  2020      18
## 6 Nayla  2021      21
```

The exact same problem is present in `B_dog_tbl` and can therefore be fixed the same way. If we want to combine our untidy `A_dog_tbl` and `B_dog_tbl` to create a tidy tibble, the code looks like this:

```r
tidyA_dog_tbl <- A_dog_tbl %>% 
  pivot_longer(c(`2020`, `2021`), names_to = "year", values_to = "height")
tidyB_dog_tbl <- B_dog_tbl %>% 
  pivot_longer(c(`2020`, `2021`), names_to = "year", values_to = "weight")
# dog and year are the matching columns across tidyA_dog_tbl 
# and tidyB_dog_tbl, so those are the variables we join by
left_join(tidyA_dog_tbl, tidyB_dog_tbl, by = c("dog", "year")) 
```

```
## # A tibble: 6 × 4
##   dog    year  height weight
##   <chr>  <chr>  <dbl>  <dbl>
## 1 Minnie 2020       8      7
## 2 Minnie 2021      12     12
## 3 Capone 2020      22     60
## 4 Capone 2021      24     75
## 5 Nayla  2020      18     32
## 6 Nayla  2021      21     40
```

#### pivot_wider
The opposite of `pivot_longer()`. It is used when an observation is scattered across multiple rows, like in the case of `dog_tbl`. Looking at `dog_tbl`, we see that each observation is spread across two rows (one observation being one dog in one year).

```r
dog_tbl
```

```
## # A tibble: 12 × 4
##    dog     year value_type value
##    <chr>  <dbl> <chr>      <dbl>
##  1 Minnie  2020 height         8
##  2 Minnie  2020 weight         7
##  3 Minnie  2021 height        12
##  4 Minnie  2021 weight        12
##  5 Capone  2020 height        22
##  6 Capone  2020 weight        60
##  7 Capone  2021 height        24
##  8 Capone  2021 weight        75
##  9 Nayla   2020 height        18
## 10 Nayla   2020 weight        32
## 11 Nayla   2021 height        21
## 12 Nayla   2021 weight        40
```

Here is how we use `pivot_wider()` to tidy `dog_tbl`

```r
# we are telling pivot_wider to get the names of the new columns from
# the "value_type" column (height and weight) and to get the values for
# these  new columns from the "value" column
dog_tbl %>%
    pivot_wider(names_from = value_type, values_from = value)
```

```
## # A tibble: 6 × 4
##   dog     year height weight
##   <chr>  <dbl>  <dbl>  <dbl>
## 1 Minnie  2020      8      7
## 2 Minnie  2021     12     12
## 3 Capone  2020     22     60
## 4 Capone  2021     24     75
## 5 Nayla   2020     18     32
## 6 Nayla   2021     21     40
```

#### separate
This function pulls one column apart into multiple columns, splitting by a given separator character. We can use it on `dog_tbl2`

```r
# another look 
dog_tbl2
```

```
## # A tibble: 6 × 3
##   dog     year hw_ratio
##   <chr>  <dbl> <chr>   
## 1 Minnie  2020 8/7     
## 2 Minnie  2021 12/12   
## 3 Capone  2020 22/60   
## 4 Capone  2021 24/75   
## 5 Nayla   2020 18/32   
## 6 Nayla   2021 21/40
```

We can separate `hw_ratio` into `height` and `weight` 

```r
# here hw_ratio is the column we want to pull apart,
# there is only one "/" in each observation, meaning we
# will separate "hw_ratio" into 2 columns, which we are telling
# separate to call "height" and "weight" - since
# height is listed first, the first part of hw_ratio will go into
# the "height" column and the second part will be
# placed in "weight"
# the sep argument tells separate which character to separate on
# this character is discarded, not placed in either new column
dog_tbl2 %>% 
  separate(hw_ratio, into = c("height", "weight"), sep = "/")
```

```
## # A tibble: 6 × 4
##   dog     year height weight
##   <chr>  <dbl> <chr>  <chr> 
## 1 Minnie  2020 8      7     
## 2 Minnie  2021 12     12    
## 3 Capone  2020 22     60    
## 4 Capone  2021 24     75    
## 5 Nayla   2020 18     32    
## 6 Nayla   2021 21     40
```
Other useful things to note is that `sep` can be multiple characters, say if you wanted to separate on "__" or even "sep_here" and if the separator character is present multiple times (a good example would be a date column formatted like 1-10-2021), the column will be separated multiple times. In the case of a date column formatted like my example, you would therefore need to name 3 new columns rather than 2, e.g. `into = c("month", "day", "year")` or `into = c("day", "month", "year")`, depending on where your data is from.

#### unite
The inverse of `separate()`. I'm going to make another (rather dumb) small tibble to demonstrate.

```r
tbl5 <- tibble(country = c("Minnie", "Minnie", "Capone", "Capone", "Nayla", "Nayla"),
               century = rep(c(20, 20), 3),
               year_in_century = rep(c(20, 21), 3))
tbl5
```

```
## # A tibble: 6 × 3
##   country century year_in_century
##   <chr>     <dbl>           <dbl>
## 1 Minnie       20              20
## 2 Minnie       20              21
## 3 Capone       20              20
## 4 Capone       20              21
## 5 Nayla        20              20
## 6 Nayla        20              21
```


```r
# fix with unite
# the first arg (year in this case) is the name of the newly-united column
# the next args (century, year_in_century) are which columns should be united
# the sep argument tells unite that I just want to smash the columns together,
# don't add a separator character (by default, unite uses "_" as the sep)
tbl5 %>% 
  unite(year, century, year_in_century, sep = "")
```

```
## # A tibble: 6 × 2
##   country year 
##   <chr>   <chr>
## 1 Minnie  2020 
## 2 Minnie  2021 
## 3 Capone  2020 
## 4 Capone  2021 
## 5 Nayla   2020 
## 6 Nayla   2021
```

### Data viz (ggplot2)

I am going to use the msleep data set to briefly introduce plotting. To make it more straightfoward, I am going to filter out any `NA` values in the data set for the variables I am going to plot.

```r
msleep <- msleep %>% 
  filter(!is.na(sleep_total)) %>% 
  filter(!is.na(sleep_rem))
```

ggplot2 builds plots in "layers" which gives you a lot of control. The very first layer is always `ggplot()`, which basically creates a plotting area for you. We can tell ggplot which data we want to use, but it won't do anything with that data without more instruction, as we can see from the blank plotting area created here.

```r
# create the plot area, give data we'll use
ggplot(data = msleep)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-59-1.png" width="672" />

We start adding "layers" with `+`. Here I am using `geom_point` to create a scatter plot. Other geom functions create other types of plots, for example `geom_boxplot()` or `geom_bar()`. 

```r
#create the plot area, give data we'll use
ggplot(data = msleep) + 
  # pick geom to plot sleep_total vs sleep_rem
  geom_point(mapping = aes(x = sleep_total, y = sleep_rem))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-60-1.png" width="672" />

Adding color with another argument inside of `aes()`

```r
ggplot(data = msleep) + 
  # add color to plot by making each data point (mammal) colored by its diet
  geom_point(mapping = aes(x = sleep_total, y = sleep_rem, color = vore)) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-61-1.png" width="672" />

Here I use `scale_color_manual()` to manually set the colors in the plot. It should be noted that if you use a fill argument, say in a boxplot or bar plot, you would need to use `scale_fill_manual()` instead.

```r
# set colorblind-friendly palette 
cbPalette <- c("#999999", "#E69F00", "#56B4E9", "#009E73", "#F0E442", "#0072B2", "#D55E00", "#CC79A7")

ggplot(data = msleep) + 
  geom_point(mapping = aes(x = sleep_total, y = sleep_rem, color = vore)) + 
  scale_color_manual(values = cbPalette) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-62-1.png" width="672" />

The `aes()` function is a mapping function, so things change with the variable if you want to apply something to the whole plot that isn't dependent on data, like making all points blue, you put the argument outside of `aes()`.

```r
ggplot(data = msleep) + 
  geom_point(mapping = aes(x = sleep_total, y = sleep_rem), color = "blue")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-63-1.png" width="672" />

Putting `color = "blue"` inside the `aes()` function does not work as expected.

```r
# just to see what happens
ggplot(data = msleep) + 
  geom_point(mapping = aes(x = sleep_total, y = sleep_rem, color = "blue"))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-64-1.png" width="672" />

Facet wraps are useful to look at many plots at once.

```r
ggplot(data = msleep) + 
  geom_point(mapping = aes(x = sleep_total, y = sleep_rem)) + 
  facet_wrap(~vore, nrow = 2) # facet wrap on class by using ~vore, so that each vore gets its own sleep_total vs sleep_rem plot - also pick 2 rows for plot arrangement
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-65-1.png" width="672" />

Finally, `theme()` is where you get a lot of customization done. I am not going to get into all the arguments of `theme()` in this post (though I'm tempted), but for quick easy plots, there are a few built-in theme functions that set a specific theme so you don't have to manually specify everything. I don't care for the default gray background in ggplots, so my favorite of these built-in theme functions is `theme_minimal()`. Here is a look at our plot using it.

```r
ggplot(data = msleep) + 
  geom_point(mapping = aes(x = sleep_total, y = sleep_rem, color = vore)) + 
  scale_color_manual(values = cbPalette) +
  theme_minimal()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-66-1.png" width="672" />


I LOVE ggplot2 and creating beautiful plots is one of my favorite parts of data science, so it was truly difficult to keep the introduction on this short. However, I don't think it will be all that helpful to go through all of geoms and possible customizations here, so I suggest looking at the ggplot2 cheatsheet on the [RStudio cheatsheets page](https://www.rstudio.com/resources/cheatsheets/) to get started in more complex plots.

