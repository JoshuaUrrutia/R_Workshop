---
layout: default
---


# Introduction to R
R is Turing Complete programming language, which means you can use R to do anything you can do in any other programming language. However, certain operations are easier to do in R and other operations are easier in other languages. For example, R is really good at working with dataframes and vectors, but is a little clunky when trying to manipulate strings of text. R is free, open-source, and has a large enthusiastic community of developers, particularly for statistics and data science applications.


## Downloading R and R Studio

Before we start working with R we will need to download R and R Studio. R Studio is an integrated development environment (IDE) for R, essentially is a application that sits on top of R and gives a nice interface for working with R.

### To install R and R Studio for windows:
*   [Download R](https://ftp.osuosl.org/pub/cran/bin/windows/base/R-3.5.0-win.exe)
*   [Download R Studio](https://download1.rstudio.org/RStudio-1.1.453.exe)

### To install R and R Studio for Mac OS X:
*   [Download R](https://ftp.osuosl.org/pub/cran/bin/macosx/R-3.5.0.pkg)
*   [Download R Studio](https://download1.rstudio.org/RStudio-1.1.453.dmg)
*   [R for Legacy Mac OS](https://ftp.osuosl.org/pub/cran/)

### Running R Studio from a Docker Container:
*   [Install Docker](https://docs.docker.com/get-docker/)
*   Run container:
```
docker run --rm -v $PWD:/home/rstudio -v ~/Downloads:/home/rstudio/Downloads -p 8787:8787 -e PASSWORD=mypassword jurrutia/rstudio:4.1.1
```
*   Navigate to http://localhost:8787 in a browser and login with the username "rstudio" and the password "mypassword" (from the command above)


### About R Studio
![R Studio IDE](../images/rstudio_info.png)
1.  In the top left we have the "Code Editor", this is a good place to construct your R Scripts and commands, once you have a workable script you can save it and resuse it later.
2.  The bottom left corner "R Console" is where the code actually gets run, it's essentially the same as using R from the command line. Code entered into the console isn't easy to export into a clear-concise script. The console can be used for quick operations (head, tail, length, etc.), that don't need to be added to the R script.
3.  The top right is the "Workspace and History Section", you can use this section to import data into R, and to see what objects have been loaded into your R session.
4.  The bottom right section "Plots and files" shows any plots that you generate, the files that are in your working directory, and also displays documentation for the R tools that are installed.


Ok, now were ready to start using R. Open up R Studio and select a working directory where'd you like to organize the files associated with this tutorial.
![R Studio SetWD](../images/rstudio-change-working-directory.png)

### R Elemental Data Types
There are five basic data types in R:
1. Numeric (1.10, 2.5, 3.2, etc)
2. Integer (1, 2, 3, etc)
3. Complex (uses imaginary numbers _i_ )
4. Logical (TRUE, FALSE)
5. Character (a, b, c, etc)

You can use the `class()` function to find out what data type you're working with. As a side note comments can be added to your R Script with the `#` symbol. Anything that follows a `#` and is on the same line will be ignored.
```r
a <- 'one'  # assigns the value 'one' the the variable a
class(a)    # prints the data type of a
b <- 2.0    # assigns the value '2.0' the the variable b
class(b)    # prints the data type of b
c <- 3      # assigns the value '3' the the variable c
class(c)    # prints the classdata type of c

a+b      # tries to add a and b
b+c      # adds b and c
c+c      # adds c to itself
```
Notice the quotes around the word `one`, what happens if you don't use the quotes? R is an object-oriented programming language, so (without quotes) it doesn't interpret the word `one` as a character, it interprets it as an object. You just created R objects a, b and c in the previous example. You can perform basic operations, like addition, on objects that have the same type. But what happens if you try to add a character and a numeric value? There is not a clear way to add a number to a character so R generates an error message. However, different data types can be combined to create collections of data: vectors, matrices, arrays, and data frames.


### Vectors
A vector is basically a list. You can make a vector that contains numbers, characters, or a combination of the two.
```r
a <- c(1,2,5.3,6,-2,4) # numeric vector
b <- c("one","two","three") # character vector
```

R uses 1-indexing (as opposed to many other languages that use 0-indexing), so to retrieve the first element of a list you can run:
```r
a[1]
b[1]
```
Or if you want particular values, you can request them explicitly by their index position:
```r
a[c(1,5)]   # returns values in the 1 and 5 position
b[c(1:3)]   # returns values from position 1, 2, and 3
```

What happens to the vector data type if you create a vector that as both characters and numbers?
```r
c <- c("one","two","three",4,5,6)
class(c)
```
The integer values get coerced into characters. So just because a value looks like a number, doesn't mean R has interpreted that value as a number. If you ever run into issues performing mathematical operations, check to make sure your data is of the proper type.

### Coercing data types
There are occasions where data will get imported as a certain type, but you will want to use a different type. You can coerce data from one type to another using the `as.datatype()` commands:
```r
a <- as.character(a)    # converts numeric a values to characters
class(a)
c <- as.numeric(c[4:6])  # converts character values from c (4-6) to numeric
class(c)
```

Be careful because not all coercions work, and some coercions can corrupt your data. For example converting from character -> numeric will not typically work.

### Data Frames

Vectors be combined into matrices, arrays, and dataframes. Matrices are two-dimensional representations (columns and rows) of elements where all the elements are same type and all columns are same length. Arrays are similar to matrices, but can have more than two dimensions. We aren't going to explore matrices or arrays in detail. Instead we'll focus on data frames.

Data frames are similar to matrices but more general; all columns in a dataframe don't have to have the same data type (you can mix numeric and characters). Let's construct and example data frame of RNA-seq counts:
```r
d <- c("KLK3","AR","CHGA","SYP") # vector of gene names
e <- c(500,200,0,0)    # vector of gene counts for LNCap
f <- c(0,0,100,300)    # vector of gene counts for LASCPC
counts <- data.frame(d,e,f)  # combines these three vectors into a dataframe
names(counts) <- c("Gene","LNCaP","LASCPC") # Changes column names to a descriptive name
```

Next we'll learn how to perform operations on dataframes.

Previous: [Main Page](../index.md) | Next: [Dataframe Operations](dataframe_ops.md) |Top: [Course Overview](../index.md)



This is a very minimal introduction, there are lots of great documentation sources for both R and R Studio. [Here is some more documentation on R](http://a-little-book-of-r-for-biomedical-statistics.readthedocs.io/en/latest/_). And R Studio maintains their own documentation [here](https://support.rstudio.com/hc/en-us/categories/200035113-Documentation).
