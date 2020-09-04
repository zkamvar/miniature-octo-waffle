---
sandpaper-digest: a3305caf223d21a0332188512e4c327f
title: Analyzing Patient Data
teaching: 45
exercises: 0
source: Rmd
---



<div class='objectives' markdown='1'>

- Read tabular data from a file into a program.
- Assign values to variables.
- Select individual values and subsections from data.
- Perform operations on a data frame of data.
- Display simple graphs.

</div>

<div class='questions' markdown='1'>

- How do I read data into R?
- How do I assign variables?
- What is a data frame?
- How do I access subsets of a data frame?
- How do I calculate simple statistics like mean and median?
- Where can I get help?
- How can I plot my data?

</div>

We are studying inflammation in patients who have been given a new treatment for arthritis,
and need to analyze the first dozen data sets.
The data sets are stored in [comma-separated values]({{ page.root }}/reference.html#comma-separated-values-csv) (CSV format. Each row holds the observations for just one patient. Each column holds the inflammation measured in a day, so we have a set of values in successive days.
The first few rows of our first file look like this:


```{.output}
0,0,1,3,1,2,4,7,8,3,3,3,10,5,7,4,7,7,12,18,6,13,11,11,7,7,4,6,8,8,4,4,5,7,3,4,2,3,0,0
0,1,2,1,2,1,3,2,2,6,10,11,5,9,4,4,7,16,8,6,18,4,12,5,12,7,11,5,11,3,3,5,4,4,5,5,1,1,0,1
0,1,1,3,3,2,6,2,5,9,5,7,4,5,4,15,5,11,9,10,19,14,12,17,7,12,11,7,4,2,10,5,4,2,2,3,2,2,1,1
0,0,2,0,4,2,2,1,6,7,10,7,9,13,8,8,15,10,10,7,17,4,4,7,6,15,6,4,9,11,3,5,6,3,3,4,2,3,2,1
0,1,1,3,3,1,3,5,2,4,4,7,6,5,3,10,8,10,6,17,9,14,9,7,13,9,12,6,7,7,9,6,3,2,2,4,2,0,1,1
```

We want to:

- Load data into memory,
- Calculate the average value of inflammation per day across all patients, and
- Plot the results.

To do all that, we'll have to learn a little bit about programming.

### Loading Data

Let's import the file called `inflammation-01.csv` into our R environment. To import the file, first we need to tell our computer where the file is. We do that by choosing a working directory, that is, a local directory on our computer containing the files we need. This is very important in R. If we forget this step we'll get an error message saying that the file does not exist. We can set the working directory using the function `setwd`. For this example, we change the path to our new directory at the desktop:


```r
setwd("~/Desktop/r-novice-inflammation/")
```

Just like in the Unix Shell, we type the command and then press <kbd>Return</kbd> (or <kbd>Enter</kbd>).
Also similar to the Unix Shell, we can use tab completion to make it easier to type the working directory. So while typing inside the setwd command's quotes, we can press <kbd>Tab</kbd> to list possible directories.
Alternatively we can change the working directory using the RStudio GUI using the menu option `Session` -> `Set Working Directory` -> `Choose Directory...`

The data file is located in the directory `data` inside the working directory. Now we can load the data into R using `read.csv`:


```r
read.csv(file = "data/inflammation-01.csv", header = FALSE)
```

The expression `read.csv(...)` is a [function call]({{ page.root }}/reference.html#function-call) that asks R to run the function `read.csv`.

`read.csv` has two [arguments]({{ page.root }}/reference.html#argument): the name of the file we want to read, and whether the first line of the file contains names for the columns of data.
The filename needs to be a character string (or [string]({{ page.root }}/reference.html#string) for short, so we put it in quotes. Assigning the second argument, `header`, to be `FALSE` indicates that the data file does not have column headers. We'll talk more about the value `FALSE`, and its converse `TRUE`, in lesson 04. In case of our `inflammation-01.csv` example, R auto-generates column names in the sequence `V1` (for "variable 1"), `V2`, and so on, until `V40`.

<div class='callout' markdown='1'>

## Other Options for Reading CSV Files
`read.csv` actually has many more arguments that you may find useful when
importing your own data in the future. You can learn more about these
options in this supplementary [lesson]({{ page.root }}/11-supp-read-write-csv/).

</div>

<div class='challenge' markdown='1'>

## Loading Data with Headers
What happens if you forget to put `header = FALSE`? The default value is
`header = TRUE`, which you can check with `?read.csv` or `help(read.csv)`.
What do you expect will happen if you leave the default value?
Before you run any code, think about what will happen to the first few rows
of your data frame, and its overall size. Then run the following code and
see if your expectations agree:

```r
read.csv(file = "data/inflammation-01.csv")
```

```{.output}
   X0 X0.1 X1 X3 X1.1 X2 X4 X7 X8 X3.1 X3.2 X3.3 X10 X5 X7.1 X4.1 X7.2 X7.3 X12 X18 X6 X13 X11 X11.1 X7.4 X7.5 X4.2 X6.1 X8.1 X8.2 X4.3 X4.4 X5.1 X7.6 X3.4 X4.5 X2.1 X3.5 X0.2 X0.3
1   0    1  2  1    2  1  3  2  2    6   10   11   5  9    4    4    7   16   8   6 18   4  12     5   12    7   11    5   11    3    3    5    4    4    5    5    1    1    0    1
2   0    1  1  3    3  2  6  2  5    9    5    7   4  5    4   15    5   11   9  10 19  14  12    17    7   12   11    7    4    2   10    5    4    2    2    3    2    2    1    1
3   0    0  2  0    4  2  2  1  6    7   10    7   9 13    8    8   15   10  10   7 17   4   4     7    6   15    6    4    9   11    3    5    6    3    3    4    2    3    2    1
4   0    1  1  3    3  1  3  5  2    4    4    7   6  5    3   10    8   10   6  17  9  14   9     7   13    9   12    6    7    7    9    6    3    2    2    4    2    0    1    1
5   0    0  1  2    2  4  2  1  6    4    7    6   6  9    9   15    4   16  18  12 12   5  18     9    5    3   10    3   12    7    8    4    7    3    5    4    4    3    2    1
6   0    0  2  2    4  2  2  5  5    8    6    5  11  9    4   13    5   12  10   6  9  17  15     8    9    3   13    7    8    2    8    8    4    2    3    5    4    1    1    1
7   0    0  1  2    3  1  2  3  5    3    7    8   8  5   10    9   15   11  18  19 20   8   5    13   15   10    6   10    6    7    4    9    3    5    2    5    3    2    2    1
8   0    0  0  3    1  5  6  5  5    8    2    4  11 12   10   11    9   10  17  11  6  16  12     6    8   14    6   13   10   11    4    6    4    7    6    3    2    1    0    0
9   0    1  1  2    1  3  5  3  5    8    6    8  12  5   13    6   13    8  16   8 18  15  16    14   12    7    3    8    9   11    2    5    4    5    1    4    1    2    0    0
10  0    1  0  0    4  3  3  5  5    4    5    8   7 10   13    3    7   13  15  18  8  15  15    16   11   14   12    4   10   10    4    3    4    5    5    3    3    2    2    1
11  0    1  0  0    3  4  2  7  8    5    2    8  11  5    5    8   14   11   6  11  9  16  18     6   12    5    4    3    5    7    8    3    5    4    5    5    4    0    1    1
12  0    0  2  1    4  3  6  4  6    7    9    9   3 11    6   12    4   17  13  15 13  12   8     7    4    7   12    9    5    6    5    4    7    3    5    4    2    3    0    1
13  0    0  0  0    1  3  1  6  6    5    5    6   3  6   13    3   10   13   9  16 15   9  11     4    6    4   11   11   12    3    5    8    7    4    6    4    1    3    0    0
14  0    1  2  1    1  1  4  1  5    2    3    3  10  7   13    5    7   17   6   9 12  13  10     4   12    4    6    7    6   10    8    2    5    1    3    4    2    0    2    0
15  0    1  1  0    1  2  4  3  6    4    7    5   5  7    5   10    7    8  18  17  9   8  12    11   11   11   14    6   11    2   10    9    5    6    5    3    4    2    2    0
16  0    0  0  0    2  3  6  5  7    4    3    2  10  7    9   11   12    5  12   9 13  19  14    17    5   13    8   11    5   10    9    8    7    5    3    1    4    0    2    1
17  0    0  0  1    2  1  4  3  6    7    4    2  12  6   12    4   14    7   8  14 13  19   6     9   12    6    4   13    6    7    2    3    6    5    4    2    3    0    1    0
18  0    0  2  1    2  5  4  2  7    8    4    7  11  9    8   11   15   17  11  12  7  12   7     6    7    4   13    5    7    6    6    9    2    1    1    2    2    0    1    0
19  0    1  2  0    1  4  3  2  2    7    3    3  12 13   11   13    6    5   9  16  9  19  16    11    8    9   14   12   11    9    6    6    6    1    1    2    4    3    1    1
20  0    1  1  3    1  4  4  1  8    2    2    3  12 12   10   15   13    6   5   5 18  19   9     6   11   12    7    6    3    6    3    2    4    3    1    5    4    2    2    0
21  0    0  2  3    2  3  2  6  3    8    7    4   6  6    9    5   12   12   8   5 12  10  16     7   14   12    5    4    6    9    8    5    6    6    1    4    3    0    2    0
22  0    0  0  3    4  5  1  7  7    8    2    5  12  4   10   14    5    5  17  13 16  15  13     6   12    9   10    3    3    7    4    4    8    2    6    5    1    0    1    0
23  0    1  1  1    1  3  3  2  6    3    9    7   8  8    4   13    7   14  11  15 14  13   5    13    7   14    9   10    5   11    5    3    5    1    1    4    4    1    2    0
24  0    1  1  1    2  3  5  3  6    3    7   10   3  8   12    4   12    9  15   5 17  16   5    10   10   15    7    5    3   11    5    5    6    1    1    1    1    0    2    1
25  0    0  2  1    3  3  2  7  4    4    3    8  12  9   12    9    5   16   8  17  7  11  14     7   13   11    7   12   12    7    8    5    7    2    2    4    1    1    1    0
26  0    0  1  2    4  2  2  3  5    7   10    5   5 12    3   13    4   13   7  15  9  12  18    14   16   12    3   11    3    2    7    4    8    2    2    1    3    0    1    1
27  0    0  1  1    1  5  1  5  2    2    4   10   4  8   14    6   15    6  12  15 15  13   7    17    4    5   11    4    8    7    9    4    5    3    2    5    4    3    2    1
28  0    0  2  2    3  4  6  3  7    6    4    5   8  4    7    7    6   11  12  19 20  18   9     5    4    7   14    8    4    3    7    7    8    3    5    4    1    3    1    0
29  0    0  0  1    4  4  6  3  8    6    4   10  12  3    3    6    8    7  17  16 14  15  17     4   14   13    4    4   12   11    6    9    5    5    2    5    2    1    0    1
30  0    1  1  0    3  2  4  6  8    6    2    3  11  3   14   14   12    8   8  16 13   7   6     9   15    7    6    4   10    8   10    4    2    6    5    5    2    3    2    1
31  0    0  2  3    3  4  5  3  6    7   10    5  10 13   14    3    8   10   9   9 19  15  15     6    8    8   11    5    5    7    3    6    6    4    5    2    2    3    0    0
32  0    1  2  2    2  3  6  6  6    7    6    3  11 12   13   15   15   10  14  11 11   8   6    12   10    5   12    7    7   11    5    8    5    2    5    5    2    0    2    1
33  0    0  2  1    3  5  6  7  5    8    9    3  12 10   12    4   12    9  13  10 10   6  10    11    4   15   13    7    3    4    2    9    7    2    4    2    1    2    1    1
34  0    0  1  2    4  1  5  5  2    3    4    8   8 12    5   15    9   17   7  19 14  18  12    17   14    4   13   13    8   11    5    6    6    2    3    5    2    1    1    1
35  0    0  0  3    1  3  6  4  3    4    8    3   4  8    3   11    5    7  10   5 15   9  16    17   16    3    8    9    8    3    3    9    5    1    6    5    4    2    2    0
36  0    1  2  2    2  5  5  1  4    6    3    6   5  9    6    7    4    7  16   7 16  13   9    16   12    6    7    9   10    3    6    4    5    4    6    3    4    3    2    1
37  0    1  1  2    3  1  5  1  2    2    5    7   6  6    5   10    6    7  17  13 15  16  17    14    4    4   10   10   10   11    9    9    5    4    4    2    1    0    1    0
38  0    1  0  3    2  4  1  1  5    9   10    7  12 10    9   15   12   13  13   6 19   9  10     6   13    5   13    6    7    2    5    5    2    1    1    1    1    3    0    1
39  0    1  1  3    1  1  5  5  3    7    2    2   3 12    4    6    8   15  16  16 15   4  14     5   13   10    7   10    6    3    2    3    6    3    3    5    4    3    2    1
40  0    0  0  2    2  1  3  4  5    5    6    5   5 12   13    5    7    5  11  15 18   7   9    10   14   12   11    9   10    3    2    9    6    2    2    5    3    0    0    1
41  0    0  1  3    3  1  2  1  8    9    2    8  10  3    8    6   10   13  11  17 19   6   4    11    6   12    7    5    5    4    4    8    2    6    6    4    2    2    0    0
42  0    1  1  3    4  5  2  1  3    7    9    6  10  5    8   15   11   12  15   6 12  16   6     4   14    3   12    9    6   11    5    8    5    5    6    1    2    1    2    0
43  0    0  1  3    1  4  3  6  7    8    5    7  11  3    6   11    6   10   6  19 18  14   6    10    7    9    8    5    8    3   10    2    5    1    5    4    2    1    0    1
44  0    1  1  3    3  4  4  6  3    4    9    9   7  6    8   15   12   15   6  11  6  18   5    14   15   12    9    8    3    6   10    6    8    7    2    5    4    3    1    1
45  0    1  2  2    4  3  1  4  8    9    5   10  10  3    4    6    7   11  16   6 14   9  11    10   10    7   10    8    8    4    5    8    4    4    5    2    4    1    1    0
46  0    0  2  3    4  5  4  6  2    9    7    4   9 10    8   11   16   12  15  17 19  10  18    13   15   11    8    4    7   11    6    7    6    5    1    3    1    0    0    0
47  0    1  1  3    1  4  6  2  8    2   10    3  11  9   13   15    5   15   6  10 10   5  14    15   12    7    4    5   11    4    6    9    5    6    1    1    2    1    2    1
48  0    0  1  3    2  5  1  2  7    6    6    3  12  9    4   14    4    6  12   9 12   7  11     7   16    8   13    6    7    6   10    7    6    3    1    5    4    3    0    0
49  0    0  1  2    3  4  5  7  5    4   10    5  12 12    5    4    7    9  18  16 16  10  15    15   10    4    3    7    5    9    4    6    2    4    1    4    2    2    2    1
50  0    1  2  1    1  3  5  3  6    3   10   10  11 10   13   10   13    6   6  14  5   4   5     5    9    4   12    7    7    4    7    9    3    3    6    3    4    1    2    0
51  0    1  2  2    3  5  2  4  5    6    8    3   5  4    3   15   15   12  16   7 20  15  12     8    9    6   12    5    8    3    8    5    4    1    3    2    1    3    1    0
52  0    0  0  2    4  4  5  3  3    3   10    4   4  4   14   11   15   13  10  14 11  17   9    11   11    7   10   12   10   10   10    8    7    5    2    2    4    1    2    1
53  0    0  2  1    1  4  4  7  2    9    4   10  12  7    6    6   11   12   9  15 15   6   6    13    5   12    9    6    4    7    7    6    5    4    1    4    2    2    2    1
54  0    1  2  1    1  4  5  4  4    5    9    7  10  3   13   13    8    9  17  16 16  15  12    13    5   12   10    9   11    9    4    5    5    2    2    5    1    0    0    1
55  0    0  1  3    2  3  6  4  5    7    2    4  11 11    3    8    8   16   5  13 16   5   8     8    6    9   10   10    9    3    3    5    3    5    4    5    3    3    0    1
56  0    1  1  2    2  5  1  7  4    2    5    5   4  6    6    4   16   11  14  16 14  14   8    17    4   14   13    7    6    3    7    7    5    6    3    4    2    2    1    1
57  0    1  1  1    4  1  6  4  6    3    6    5   6  4   14   13   13    9  12  19  9  10  15    10    9   10   10    7    5    6    8    6    6    4    3    5    2    1    1    1
58  0    0  0  1    4  5  6  3  8    7    9   10   8  6    5   12   15    5  10   5  8  13  18    17   14    9   13    4   10   11   10    8    8    6    5    5    2    0    2    0
59  0    0  1  0    3  2  5  4  8    2    9    3   3 10   12    9   14   11  13   8  6  18  11     9   13   11    8    5    5    2    8    5    3    5    4    1    3    1    1    0
```

<div class='solution' markdown='1'>

## Solution
 
R will construct column headers from values in your first row of data,
resulting in `X0 X0.1 X1 X3 X1.1 X2 ...`.

Note that the `X` is prepended just a number would not be a valid variable
name. Because that's what column headers are, the same rules apply.
Appending `.1`, `.2` etc. is necessary to avoid duplicate column headers.


</div>

</div>

<div class='challenge' markdown='1'>

## Reading Different Decimal Point Formats
Depending on the country you live in, your standard can use the dot or the comma as decimal mark.
Also, different devices or software can generate data with different decimal points.
Take a look at `?read.csv` and write the code to load a file called `commadec.txt` that has numeric values with commas as decimal mark, separated by semicolons.

<div class='solution' markdown='1'>

## Solution
 

```r
read.csv(file = "data/commadec.txt", sep = ";", dec = ",")
```

```{.warning}
Warning in file(file, "rt"): cannot open file 'data/commadec.txt': No such file or directory
```

```{.error}
Error in file(file, "rt"): cannot open the connection
```

or the built-in shortcut:

```r
read.csv2(file = "data/commadec.txt")
```

```{.warning}
Warning in file(file, "rt"): cannot open file 'data/commadec.txt': No such file or directory
```

```{.error}
Error in file(file, "rt"): cannot open the connection
```

</div>

</div>

A function will perform its given action on whatever value is passed to the argument(s).
For example, in this case if we provided the name of a different file to the argument `file`, `read.csv` would read that instead.
We'll learn more about the details of functions and their arguments in the next lesson.

Since we didn't tell it to do anything else with the function's output, the console will display the full contents of the file `inflammation-01.csv`.
Try it out.

`read.csv` reads the file, but we can't use the data unless we assign it to a variable.
We can think of a variable as a container with a name, such as `x`, `current_temperature`, or `subject_id` that contains one or more values.
We can create a new variable and assign a value to it using `<-`.


```r
weight_kg <- 55
```

Once a variable is created, we can use the variable name to refer to the value it was assigned. The variable name now acts as a tag. Whenever R reads that tag (`weight_kg`), it substitutes the value (`55`).

<img src="fig/tag-variables.svg" alt="Variables as Tags" />

To see the value of a variable, we can print it by typing the name of the variable and hitting <kbd>Return</kbd> (or <kbd>Enter</kbd>).
In general, R will print to the console any object returned by a function or operation *unless* we assign it to a variable.


```r
weight_kg
```

```{.output}
[1] 55
```

We can treat our variable like a regular number, and do arithmetic with it:


```r
# weight in pounds:
2.2 * weight_kg
```

```{.output}
[1] 121
```

<img src="fig/arithmetic-variables.svg" alt="Variables as Tags" />

<div class='callout' markdown='1'>

## Commenting
We can add comments to our code using the `#` character. It is useful to
document our code in this way so that others (and us the next time we
read it) have an easier time following what the code is doing.

</div>

We can also change a variable's value by assigning it a new value:


```r
weight_kg <- 57.5
# weight in kilograms is now
weight_kg
```

```{.output}
[1] 57.5
```

<div class='callout' markdown='1'>

## Variable Naming Conventions
Historically, R programmers have used a variety of conventions for naming variables. The `.` character
in R can be a valid part of a variable name; thus the above assignment could have easily been `weight.kg <- 57.5`.
This is often confusing to R newcomers who have programmed in languages where `.` has a more significant meaning.
Today, most R programmers 1) start variable names with lower case letters, 2) separate words in variable names with
underscores, and 3) use only lowercase letters, underscores, and numbers in variable names. The book *R Packages* includes
a [chapter](http://r-pkgs.had.co.nz/style.html) on this and other style considerations.

</div>

<img src="fig/reassign-variables.svg" alt="Reassigning Variables" />

Assigning a new value to a variable breaks the connection with the old value; R forgets that number and applies the variable name to the new value.

When you assign a value to a variable, R only stores the value, not the calculation you used to create it. This is an important point if you're used to the way a spreadsheet program automatically updates linked cells. Let's look at an example.

First, we'll convert `weight_kg` into pounds, and store the new value in the variable `weight_lb`:


```r
weight_lb <- 2.2 * weight_kg
# weight in kg...
weight_kg
```

```{.output}
[1] 57.5
```

```r
# ...and in pounds
weight_lb
```

```{.output}
[1] 126.5
```

In words, we're asking R to look up the value we tagged `weight_kg`,
multiply it by 2.2, and tag the result with the name `weight_lb`:

<img src="fig/new-variables.svg" alt="Creating Another Variable" />

If we now change the value of `weight_kg`:


```r
weight_kg <- 100.0
# weight in kg now...
weight_kg
```

```{.output}
[1] 100
```

```r
# ...and weight in pounds still
weight_lb
```

```{.output}
[1] 126.5
```

<img src="fig/memory-variables.svg" alt="Updating a Variable" />

Since `weight_lb` doesn't "remember" where its value came from, it isn't automatically updated when `weight_kg` changes.
This is different from the way spreadsheets work.

<div class='callout' markdown='1'>

## Printing with Parentheses
An alternative way to print the value of a variable is to use
`(` parentheses `)` around the assignment statement.
As an example: `(total_weight <- weight_kg*2.2 + weight_lb)` adds the values of `weight_kg*2.2` and `weight_lb`,
assigns the result to the `total_weight`,
and finally prints the assigned value of the variable `total_weight`.

</div>

Now that we know how to assign things to variables, let's re-run `read.csv` and save its result into a variable called 'dat':


```r
dat <- read.csv(file = "data/inflammation-01.csv", header = FALSE)
```

This statement doesn't produce any output because the assignment doesn't display anything.
If we want to check if our data has been loaded, we can print the variable's value by typing the name of the variable `dat`. However, for large data sets it is convenient to use the function `head` to display only the first few rows of data.


```r
head(dat)
```

```{.output}
  V1 V2 V3 V4 V5 V6 V7 V8 V9 V10 V11 V12 V13 V14 V15 V16 V17 V18 V19 V20 V21 V22 V23 V24 V25 V26 V27 V28 V29 V30 V31 V32 V33 V34 V35 V36 V37 V38 V39 V40
1  0  0  1  3  1  2  4  7  8   3   3   3  10   5   7   4   7   7  12  18   6  13  11  11   7   7   4   6   8   8   4   4   5   7   3   4   2   3   0   0
2  0  1  2  1  2  1  3  2  2   6  10  11   5   9   4   4   7  16   8   6  18   4  12   5  12   7  11   5  11   3   3   5   4   4   5   5   1   1   0   1
3  0  1  1  3  3  2  6  2  5   9   5   7   4   5   4  15   5  11   9  10  19  14  12  17   7  12  11   7   4   2  10   5   4   2   2   3   2   2   1   1
4  0  0  2  0  4  2  2  1  6   7  10   7   9  13   8   8  15  10  10   7  17   4   4   7   6  15   6   4   9  11   3   5   6   3   3   4   2   3   2   1
5  0  1  1  3  3  1  3  5  2   4   4   7   6   5   3  10   8  10   6  17   9  14   9   7  13   9  12   6   7   7   9   6   3   2   2   4   2   0   1   1
6  0  0  1  2  2  4  2  1  6   4   7   6   6   9   9  15   4  16  18  12  12   5  18   9   5   3  10   3  12   7   8   4   7   3   5   4   4   3   2   1
```

<div class='challenge' markdown='1'>

## Assigning Values to Variables
Draw diagrams showing what variables refer to what values after each statement in the following program:

```r
mass <- 47.5
age <- 122
mass <- mass * 2.0
age <- age - 20
```

<div class='solution' markdown='1'>

## Solution
 

```r
mass <- 47.5
age <- 122
```

<img src="fig/mass-age-assign-1.svg" alt="Assigning Variables" />

```r
mass <- mass * 2.0
age <- age - 20
```

<img src="fig/mass-age-assign-2.svg" alt="Assigning Variables" />

</div>

</div>

### Manipulating Data

Now that our data are loaded into R, we can start doing things with them.
First, let's ask what type of thing `dat` is:


```r
class(dat)
```

```{.output}
[1] "data.frame"
```

The output tells us that it's a data frame. Think of this structure as a spreadsheet in MS Excel that many of us are familiar with.
Data frames are very useful for storing data and you will use them frequently when programming in R.
A typical data frame of experimental data contains individual observations in rows and variables in columns.

We can see the shape, or [dimensions]({{ page.root }}/reference.html#dimensions-of-an-array), of the data frame with the function `dim`:


```r
dim(dat)
```

```{.output}
[1] 60 40
```

This tells us that our data frame, `dat`, has 60 rows and 40 columns.

If we want to get a single value from the data frame, we can provide an [index]({{ page.root }}/reference.html#index) in square brackets. The first number specifies the row and the second the column:


```r
# first value in dat, row 1, column 1
dat[1, 1]
```

```{.output}
[1] 0
```

```r
# middle value in dat, row 30, column 20
dat[30, 20]
```

```{.output}
[1] 16
```

The first value in a data frame index is the row, the second value is the column.
If we want to select more than one row or column, we can use the function `c`, which stands for **c**ombine.
For example, to pick columns 10 and 20 from rows 1, 3, and 5, we can do this:


```r
dat[c(1, 3, 5), c(10, 20)]
```

```{.output}
  V10 V20
1   3  18
3   9  10
5   4  17
```

We frequently want to select contiguous rows or columns, such as the first ten rows, or columns 3 through 7. You can use `c` for this, but it's more convenient to use the `:` operator. This special function generates sequences of numbers:


```r
1:5
```

```{.output}
[1] 1 2 3 4 5
```

```r
3:12
```

```{.output}
 [1]  3  4  5  6  7  8  9 10 11 12
```

For example, we can select the first ten columns of values for the first four rows like this:


```r
dat[1:4, 1:10]
```

```{.output}
  V1 V2 V3 V4 V5 V6 V7 V8 V9 V10
1  0  0  1  3  1  2  4  7  8   3
2  0  1  2  1  2  1  3  2  2   6
3  0  1  1  3  3  2  6  2  5   9
4  0  0  2  0  4  2  2  1  6   7
```

or the first ten columns of rows 5 to 10 like this:


```r
dat[5:10, 1:10]
```

```{.output}
   V1 V2 V3 V4 V5 V6 V7 V8 V9 V10
5   0  1  1  3  3  1  3  5  2   4
6   0  0  1  2  2  4  2  1  6   4
7   0  0  2  2  4  2  2  5  5   8
8   0  0  1  2  3  1  2  3  5   3
9   0  0  0  3  1  5  6  5  5   8
10  0  1  1  2  1  3  5  3  5   8
```

If you want to select all rows or all columns, leave that index value empty.


```r
# All columns from row 5
dat[5, ]
```

```{.output}
  V1 V2 V3 V4 V5 V6 V7 V8 V9 V10 V11 V12 V13 V14 V15 V16 V17 V18 V19 V20 V21 V22 V23 V24 V25 V26 V27 V28 V29 V30 V31 V32 V33 V34 V35 V36 V37 V38 V39 V40
5  0  1  1  3  3  1  3  5  2   4   4   7   6   5   3  10   8  10   6  17   9  14   9   7  13   9  12   6   7   7   9   6   3   2   2   4   2   0   1   1
```

```r
# All rows from column 16-18
dat[, 16:18]
```

```{.output}
   V16 V17 V18
1    4   7   7
2    4   7  16
3   15   5  11
4    8  15  10
5   10   8  10
6   15   4  16
7   13   5  12
8    9  15  11
9   11   9  10
10   6  13   8
11   3   7  13
12   8  14  11
13  12   4  17
14   3  10  13
15   5   7  17
16  10   7   8
17  11  12   5
18   4  14   7
19  11  15  17
20  13   6   5
21  15  13   6
22   5  12  12
23  14   5   5
24  13   7  14
25   4  12   9
26   9   5  16
27  13   4  13
28   6  15   6
29   7   6  11
30   6   8   7
31  14  12   8
32   3   8  10
33  15  15  10
34   4  12   9
35  15   9  17
36  11   5   7
37   7   4   7
38  10   6   7
39  15  12  13
40   6   8  15
41   5   7   5
42   6  10  13
43  15  11  12
44  11   6  10
45  15  12  15
46   6   7  11
47  11  16  12
48  15   5  15
49  14   4   6
50   4   7   9
51  10  13   6
52  15  15  12
53  11  15  13
54   6  11  12
55  13   8   9
56   8   8  16
57   4  16  11
58  13  13   9
59  12  15   5
60   9  14  11
```

If you leave both index values empty (i.e., `dat[,]`), you get the entire data frame.

<div class='callout' markdown='1'>

## Addressing Columns by Name
Columns can also be addressed by name, with either the `$` operator (ie. `dat$V16`) or square brackets (ie. `dat[, 'V16']`).
You can learn more about subsetting by column name in this supplementary [lesson]({{ page.root }}/10-supp-addressing-data/).

</div>

Now let's perform some common mathematical operations to learn more about our inflammation data.
When analyzing data we often want to look at partial statistics, such as the maximum value per patient or the average value per day.
One way to do this is to select the data we want to create a new temporary data frame, and then perform the calculation on this subset:


```r
# first row, all of the columns
patient_1 <- dat[1, ]
# max inflammation for patient 1
max(patient_1)
```

```{.output}
[1] 18
```

<!-- 
    OUCH!! The following may be true, but it will vary by R version, not by
    installation! There shouldn't be an issue with a data frame where all
    columns are numeric without missing values. Under those circumstances,
    coercion should do what you expect. You'll get problems with mixed
    types (factors, character etc), or with missing values. If this is
    actually a problem, we need to change the example - we should be able
    to come up with an example that doesn't require this ugliness in the
    very first lesson. 
    
    Also, columns always work as expected because by definition a column
    contains a vector of values of the same type. Rows include values from
    different columns, which of course can be different types. This doesn't
    need to be confusing, and if we're careful in our presentation here we
    can avoid this until the students know enough to cope rationally.
-->

<!--
> ## Forcing Conversion
>
> The code above may give you an error in some R installations,
> since R does not automatically convert a row from a `data.frame` to a vector.
> (Confusingly, subsetted columns are automatically converted.)
> If this happens, you can use the `as.numeric` command to convert the row of data to a numeric vector:
>
> `patient_1 <- as.numeric(dat[1, ])`
>
> `max(patient_1)`
>
> You can also check the `class` of each object:
>
> `class(dat[1, ])`
>
> `class(as.numeric(dat[1, ]))`
{: .callout}
-->

We don't actually need to store the row in a variable of its own.
Instead, we can combine the selection and the function call:


```r
# max inflammation for patient 2
max(dat[2, ])
```

```{.output}
[1] 18
```

R also has functions for other common calculations, e.g. finding the minimum, mean, median, and standard deviation of the data:


```r
# minimum inflammation on day 7
min(dat[, 7])
```

```{.output}
[1] 1
```

```r
# mean inflammation on day 7
mean(dat[, 7])
```

```{.output}
[1] 3.8
```

```r
# median inflammation on day 7
median(dat[, 7])
```

```{.output}
[1] 4
```

```r
# standard deviation of inflammation on day 7
sd(dat[, 7])
```

```{.output}
[1] 1.725187
```

<div class='callout' markdown='1'>

## Forcing Conversion
Note that R may return an error when you attempt to perform similar calculations on
subsetted *rows* of data frames. This is because some functions in R automatically convert
the object type to a numeric vector, while others do not (e.g. `max(dat[1, ])` works as
expected, while `mean(dat[1, ])` returns `NA` and a warning). You get the
expected output by including an
explicit call to `as.numeric()`, e.g. `mean(as.numeric(dat[1, ]))`. By contrast,
calculations on subsetted *columns* always work as expected, since columns of data frames
are already defined as vectors.

</div>

R also has a function that summaries the previous common calculations:


```r
# Summarize function
summary(dat[, 1:4])
```

```{.output}
       V1          V2             V3              V4      
 Min.   :0   Min.   :0.00   Min.   :0.000   Min.   :0.00  
 1st Qu.:0   1st Qu.:0.00   1st Qu.:1.000   1st Qu.:1.00  
 Median :0   Median :0.00   Median :1.000   Median :2.00  
 Mean   :0   Mean   :0.45   Mean   :1.117   Mean   :1.75  
 3rd Qu.:0   3rd Qu.:1.00   3rd Qu.:2.000   3rd Qu.:3.00  
 Max.   :0   Max.   :1.00   Max.   :2.000   Max.   :3.00  
```

For every column in the data frame, the function "summary" calculates: the minimun value, the first quartile, the median, the mean, the third quartile and the max value, giving helpful details about the sample distribution.

What if we need the maximum inflammation for all patients, or the average for each day?
As the diagram below shows, we want to perform the operation across a margin of the data frame:

<img src="fig/r-operations-across-margins.svg" alt="Operations Across Margins" />

To support this, we can use the `apply` function.

<div class='callout' markdown='1'>

## Getting Help
To learn about a function in R, e.g. `apply`, we can read its help
documention by running `help(apply)` or `?apply`.

</div>

`apply` allows us to repeat a function on all of the rows (`MARGIN = 1`) or columns (`MARGIN = 2`) of a data frame.

Thus, to obtain the average inflammation of each patient we will need to calculate the mean of all of the rows (`MARGIN = 1`) of the data frame.


```r
avg_patient_inflammation <- apply(dat, 1, mean)
```

And to obtain the average inflammation of each day we will need to calculate the mean of all of the columns (`MARGIN = 2`) of the data frame.


```r
avg_day_inflammation <- apply(dat, 2, mean)
```

Since the second argument to `apply` is `MARGIN`, the above command is equivalent to `apply(dat, MARGIN = 2, mean)`.
We'll learn why this is so in the next lesson.

<div class='callout' markdown='1'>

## Efficient Alternatives
Some common operations have more efficient alternatives. For example, you
can calculate the row-wise or column-wise means with `rowMeans` and
`colMeans`, respectively.

</div>

<div class='challenge' markdown='1'>

## Subsetting Data
We can take subsets of character vectors as well:

```r
animal <- c("m", "o", "n", "k", "e", "y")
# first three characters
animal[1:3]
```

```{.output}
[1] "m" "o" "n"
```

```r
# last three characters
animal[4:6]
```

```{.output}
[1] "k" "e" "y"
```

1. If the first four characters are selected using the subset `animal[1:4]`, how can we obtain the first four characters in reverse order?

2. What is `animal[-1]`?
   What is `animal[-4]`?
   Given those answers,
   explain what `animal[-1:-4]` does.

3. Use a subset of `animal` to create a new character vector that spells the word "eon", i.e. `c("e", "o", "n")`.

<div class='solution' markdown='1'>

## Solutions
 
1. `animal[4:1]`

2. `"o" "n" "k" "e" "y"` and `"m" "o" "n" "e" "y"`, which means that a
   single `-` removes the element at the given index position.
   `animal[-1:-4]` remove the subset, returning `"e" "y"`, which is
   equivalent to `animal[5:6]`.

3. `animal[c(5,2,3)]` combines indexing with the `c`ombine function.

</div>

</div>

<div class='challenge' markdown='1'>

## Subsetting More Data
Suppose you want to determine the maximum inflammation for patient 5 across days three to seven.
To do this you would extract the relevant subset from the data frame and calculate the maximum value.
Which of the following lines of R code gives the correct answer?
1. `max(dat[5, ])`
2. `max(dat[3:7, 5])`
3. `max(dat[5, 3:7])`
4. `max(dat[5, 3, 7])`

<div class='solution' markdown='1'>

## Solution
 
Answer: 3

Explanation: You want to extract the part of the dataframe representing data for patient 5 from days three to seven. In this dataframe, patient data is organised in rows and the days are represented by the columns. Subscripting in R follows the `[i, j]` principle, where `i = rows` and `j = columns`. Thus, answer 3 is correct since the patient is represented by the value for i (5) and the days are represented by the values in j, which is a subset spanning day 3 to 7.

</div>

</div>

<div class='challenge' markdown='1'>

## Subsetting and Re-Assignment
Using the inflammation data frame `dat` from above:
Let's pretend there was something wrong with the instrument on the first five days for every second patient (#2, 4, 6, etc.), which resulted in the measurements being twice as large as they should be.
1. Write a vector containing each affected patient (hint: `?seq`)
2. Create a new data frame in which you halve the first five days' values in only those patients
3. Print out the corrected data frame to check that your code has fixed the problem

<div class='solution' markdown='1'>

## Solution
 

```r
whichPatients <- seq(2, 60, 2) # i.e., which rows
whichDays <- seq(1, 5)         # i.e., which columns
dat2 <- dat
# check the size of your subset: returns `30 5`, that is 30 [rows=patients] by 5 [columns=days]
dim(dat2[whichPatients, whichDays])
```

```{.output}
[1] 30  5
```

```r
dat2[whichPatients, whichDays] <- dat2[whichPatients, whichDays] / 2
dat2
```

```{.output}
   V1  V2  V3  V4  V5 V6 V7 V8 V9 V10 V11 V12 V13 V14 V15 V16 V17 V18 V19 V20 V21 V22 V23 V24 V25 V26 V27 V28 V29 V30 V31 V32 V33 V34 V35 V36 V37 V38 V39 V40
1   0 0.0 1.0 3.0 1.0  2  4  7  8   3   3   3  10   5   7   4   7   7  12  18   6  13  11  11   7   7   4   6   8   8   4   4   5   7   3   4   2   3   0   0
2   0 0.5 1.0 0.5 1.0  1  3  2  2   6  10  11   5   9   4   4   7  16   8   6  18   4  12   5  12   7  11   5  11   3   3   5   4   4   5   5   1   1   0   1
3   0 1.0 1.0 3.0 3.0  2  6  2  5   9   5   7   4   5   4  15   5  11   9  10  19  14  12  17   7  12  11   7   4   2  10   5   4   2   2   3   2   2   1   1
4   0 0.0 1.0 0.0 2.0  2  2  1  6   7  10   7   9  13   8   8  15  10  10   7  17   4   4   7   6  15   6   4   9  11   3   5   6   3   3   4   2   3   2   1
5   0 1.0 1.0 3.0 3.0  1  3  5  2   4   4   7   6   5   3  10   8  10   6  17   9  14   9   7  13   9  12   6   7   7   9   6   3   2   2   4   2   0   1   1
6   0 0.0 0.5 1.0 1.0  4  2  1  6   4   7   6   6   9   9  15   4  16  18  12  12   5  18   9   5   3  10   3  12   7   8   4   7   3   5   4   4   3   2   1
7   0 0.0 2.0 2.0 4.0  2  2  5  5   8   6   5  11   9   4  13   5  12  10   6   9  17  15   8   9   3  13   7   8   2   8   8   4   2   3   5   4   1   1   1
8   0 0.0 0.5 1.0 1.5  1  2  3  5   3   7   8   8   5  10   9  15  11  18  19  20   8   5  13  15  10   6  10   6   7   4   9   3   5   2   5   3   2   2   1
9   0 0.0 0.0 3.0 1.0  5  6  5  5   8   2   4  11  12  10  11   9  10  17  11   6  16  12   6   8  14   6  13  10  11   4   6   4   7   6   3   2   1   0   0
10  0 0.5 0.5 1.0 0.5  3  5  3  5   8   6   8  12   5  13   6  13   8  16   8  18  15  16  14  12   7   3   8   9  11   2   5   4   5   1   4   1   2   0   0
11  0 1.0 0.0 0.0 4.0  3  3  5  5   4   5   8   7  10  13   3   7  13  15  18   8  15  15  16  11  14  12   4  10  10   4   3   4   5   5   3   3   2   2   1
12  0 0.5 0.0 0.0 1.5  4  2  7  8   5   2   8  11   5   5   8  14  11   6  11   9  16  18   6  12   5   4   3   5   7   8   3   5   4   5   5   4   0   1   1
13  0 0.0 2.0 1.0 4.0  3  6  4  6   7   9   9   3  11   6  12   4  17  13  15  13  12   8   7   4   7  12   9   5   6   5   4   7   3   5   4   2   3   0   1
14  0 0.0 0.0 0.0 0.5  3  1  6  6   5   5   6   3   6  13   3  10  13   9  16  15   9  11   4   6   4  11  11  12   3   5   8   7   4   6   4   1   3   0   0
15  0 1.0 2.0 1.0 1.0  1  4  1  5   2   3   3  10   7  13   5   7  17   6   9  12  13  10   4  12   4   6   7   6  10   8   2   5   1   3   4   2   0   2   0
16  0 0.5 0.5 0.0 0.5  2  4  3  6   4   7   5   5   7   5  10   7   8  18  17   9   8  12  11  11  11  14   6  11   2  10   9   5   6   5   3   4   2   2   0
17  0 0.0 0.0 0.0 2.0  3  6  5  7   4   3   2  10   7   9  11  12   5  12   9  13  19  14  17   5  13   8  11   5  10   9   8   7   5   3   1   4   0   2   1
18  0 0.0 0.0 0.5 1.0  1  4  3  6   7   4   2  12   6  12   4  14   7   8  14  13  19   6   9  12   6   4  13   6   7   2   3   6   5   4   2   3   0   1   0
19  0 0.0 2.0 1.0 2.0  5  4  2  7   8   4   7  11   9   8  11  15  17  11  12   7  12   7   6   7   4  13   5   7   6   6   9   2   1   1   2   2   0   1   0
20  0 0.5 1.0 0.0 0.5  4  3  2  2   7   3   3  12  13  11  13   6   5   9  16   9  19  16  11   8   9  14  12  11   9   6   6   6   1   1   2   4   3   1   1
21  0 1.0 1.0 3.0 1.0  4  4  1  8   2   2   3  12  12  10  15  13   6   5   5  18  19   9   6  11  12   7   6   3   6   3   2   4   3   1   5   4   2   2   0
22  0 0.0 1.0 1.5 1.0  3  2  6  3   8   7   4   6   6   9   5  12  12   8   5  12  10  16   7  14  12   5   4   6   9   8   5   6   6   1   4   3   0   2   0
23  0 0.0 0.0 3.0 4.0  5  1  7  7   8   2   5  12   4  10  14   5   5  17  13  16  15  13   6  12   9  10   3   3   7   4   4   8   2   6   5   1   0   1   0
24  0 0.5 0.5 0.5 0.5  3  3  2  6   3   9   7   8   8   4  13   7  14  11  15  14  13   5  13   7  14   9  10   5  11   5   3   5   1   1   4   4   1   2   0
25  0 1.0 1.0 1.0 2.0  3  5  3  6   3   7  10   3   8  12   4  12   9  15   5  17  16   5  10  10  15   7   5   3  11   5   5   6   1   1   1   1   0   2   1
26  0 0.0 1.0 0.5 1.5  3  2  7  4   4   3   8  12   9  12   9   5  16   8  17   7  11  14   7  13  11   7  12  12   7   8   5   7   2   2   4   1   1   1   0
27  0 0.0 1.0 2.0 4.0  2  2  3  5   7  10   5   5  12   3  13   4  13   7  15   9  12  18  14  16  12   3  11   3   2   7   4   8   2   2   1   3   0   1   1
28  0 0.0 0.5 0.5 0.5  5  1  5  2   2   4  10   4   8  14   6  15   6  12  15  15  13   7  17   4   5  11   4   8   7   9   4   5   3   2   5   4   3   2   1
29  0 0.0 2.0 2.0 3.0  4  6  3  7   6   4   5   8   4   7   7   6  11  12  19  20  18   9   5   4   7  14   8   4   3   7   7   8   3   5   4   1   3   1   0
30  0 0.0 0.0 0.5 2.0  4  6  3  8   6   4  10  12   3   3   6   8   7  17  16  14  15  17   4  14  13   4   4  12  11   6   9   5   5   2   5   2   1   0   1
31  0 1.0 1.0 0.0 3.0  2  4  6  8   6   2   3  11   3  14  14  12   8   8  16  13   7   6   9  15   7   6   4  10   8  10   4   2   6   5   5   2   3   2   1
32  0 0.0 1.0 1.5 1.5  4  5  3  6   7  10   5  10  13  14   3   8  10   9   9  19  15  15   6   8   8  11   5   5   7   3   6   6   4   5   2   2   3   0   0
33  0 1.0 2.0 2.0 2.0  3  6  6  6   7   6   3  11  12  13  15  15  10  14  11  11   8   6  12  10   5  12   7   7  11   5   8   5   2   5   5   2   0   2   1
34  0 0.0 1.0 0.5 1.5  5  6  7  5   8   9   3  12  10  12   4  12   9  13  10  10   6  10  11   4  15  13   7   3   4   2   9   7   2   4   2   1   2   1   1
35  0 0.0 1.0 2.0 4.0  1  5  5  2   3   4   8   8  12   5  15   9  17   7  19  14  18  12  17  14   4  13  13   8  11   5   6   6   2   3   5   2   1   1   1
36  0 0.0 0.0 1.5 0.5  3  6  4  3   4   8   3   4   8   3  11   5   7  10   5  15   9  16  17  16   3   8   9   8   3   3   9   5   1   6   5   4   2   2   0
37  0 1.0 2.0 2.0 2.0  5  5  1  4   6   3   6   5   9   6   7   4   7  16   7  16  13   9  16  12   6   7   9  10   3   6   4   5   4   6   3   4   3   2   1
38  0 0.5 0.5 1.0 1.5  1  5  1  2   2   5   7   6   6   5  10   6   7  17  13  15  16  17  14   4   4  10  10  10  11   9   9   5   4   4   2   1   0   1   0
39  0 1.0 0.0 3.0 2.0  4  1  1  5   9  10   7  12  10   9  15  12  13  13   6  19   9  10   6  13   5  13   6   7   2   5   5   2   1   1   1   1   3   0   1
40  0 0.5 0.5 1.5 0.5  1  5  5  3   7   2   2   3  12   4   6   8  15  16  16  15   4  14   5  13  10   7  10   6   3   2   3   6   3   3   5   4   3   2   1
41  0 0.0 0.0 2.0 2.0  1  3  4  5   5   6   5   5  12  13   5   7   5  11  15  18   7   9  10  14  12  11   9  10   3   2   9   6   2   2   5   3   0   0   1
42  0 0.0 0.5 1.5 1.5  1  2  1  8   9   2   8  10   3   8   6  10  13  11  17  19   6   4  11   6  12   7   5   5   4   4   8   2   6   6   4   2   2   0   0
43  0 1.0 1.0 3.0 4.0  5  2  1  3   7   9   6  10   5   8  15  11  12  15   6  12  16   6   4  14   3  12   9   6  11   5   8   5   5   6   1   2   1   2   0
44  0 0.0 0.5 1.5 0.5  4  3  6  7   8   5   7  11   3   6  11   6  10   6  19  18  14   6  10   7   9   8   5   8   3  10   2   5   1   5   4   2   1   0   1
45  0 1.0 1.0 3.0 3.0  4  4  6  3   4   9   9   7   6   8  15  12  15   6  11   6  18   5  14  15  12   9   8   3   6  10   6   8   7   2   5   4   3   1   1
46  0 0.5 1.0 1.0 2.0  3  1  4  8   9   5  10  10   3   4   6   7  11  16   6  14   9  11  10  10   7  10   8   8   4   5   8   4   4   5   2   4   1   1   0
47  0 0.0 2.0 3.0 4.0  5  4  6  2   9   7   4   9  10   8  11  16  12  15  17  19  10  18  13  15  11   8   4   7  11   6   7   6   5   1   3   1   0   0   0
48  0 0.5 0.5 1.5 0.5  4  6  2  8   2  10   3  11   9  13  15   5  15   6  10  10   5  14  15  12   7   4   5  11   4   6   9   5   6   1   1   2   1   2   1
49  0 0.0 1.0 3.0 2.0  5  1  2  7   6   6   3  12   9   4  14   4   6  12   9  12   7  11   7  16   8  13   6   7   6  10   7   6   3   1   5   4   3   0   0
50  0 0.0 0.5 1.0 1.5  4  5  7  5   4  10   5  12  12   5   4   7   9  18  16  16  10  15  15  10   4   3   7   5   9   4   6   2   4   1   4   2   2   2   1
51  0 1.0 2.0 1.0 1.0  3  5  3  6   3  10  10  11  10  13  10  13   6   6  14   5   4   5   5   9   4  12   7   7   4   7   9   3   3   6   3   4   1   2   0
52  0 0.5 1.0 1.0 1.5  5  2  4  5   6   8   3   5   4   3  15  15  12  16   7  20  15  12   8   9   6  12   5   8   3   8   5   4   1   3   2   1   3   1   0
53  0 0.0 0.0 2.0 4.0  4  5  3  3   3  10   4   4   4  14  11  15  13  10  14  11  17   9  11  11   7  10  12  10  10  10   8   7   5   2   2   4   1   2   1
54  0 0.0 1.0 0.5 0.5  4  4  7  2   9   4  10  12   7   6   6  11  12   9  15  15   6   6  13   5  12   9   6   4   7   7   6   5   4   1   4   2   2   2   1
55  0 1.0 2.0 1.0 1.0  4  5  4  4   5   9   7  10   3  13  13   8   9  17  16  16  15  12  13   5  12  10   9  11   9   4   5   5   2   2   5   1   0   0   1
56  0 0.0 0.5 1.5 1.0  3  6  4  5   7   2   4  11  11   3   8   8  16   5  13  16   5   8   8   6   9  10  10   9   3   3   5   3   5   4   5   3   3   0   1
57  0 1.0 1.0 2.0 2.0  5  1  7  4   2   5   5   4   6   6   4  16  11  14  16  14  14   8  17   4  14  13   7   6   3   7   7   5   6   3   4   2   2   1   1
58  0 0.5 0.5 0.5 2.0  1  6  4  6   3   6   5   6   4  14  13  13   9  12  19   9  10  15  10   9  10  10   7   5   6   8   6   6   4   3   5   2   1   1   1
59  0 0.0 0.0 1.0 4.0  5  6  3  8   7   9  10   8   6   5  12  15   5  10   5   8  13  18  17  14   9  13   4  10  11  10   8   8   6   5   5   2   0   2   0
60  0 0.0 0.5 0.0 1.5  2  5  4  8   2   9   3   3  10  12   9  14  11  13   8   6  18  11   9  13  11   8   5   5   2   8   5   3   5   4   1   3   1   1   0
```

</div>

</div>

<div class='challenge' markdown='1'>

## Using the Apply Function on Patient Data
Challenge: the apply function can be used to summarize datasets and subsets
of data across rows and columns using the MARGIN argument.
Suppose you want to calculate the mean inflammation for specific days and patients
in the patient dataset (i.e. 60 patients across 40 days).
Please use a combination of the apply function and indexing to:

1. calculate the mean inflammation for patients 1 to 5 over the whole 40 days
2. calculate the mean inflammation for days 1 to 10 (across all patients).
3. calculate the mean inflammation for every second day (across all patients).

Think about the number of rows and columns you would expect as the result before each
apply call and check your intuition by applying the mean function.

<div class='solution' markdown='1'>

## Solution
 

```r
# 1.
apply(dat[1:5, ], 1, mean)
```

```{.output}
    1     2     3     4     5 
5.450 5.425 6.100 5.900 5.550 
```

```r
# 2.
apply(dat[, 1:10], 2, mean)
```

```{.output}
      V1       V2       V3       V4       V5       V6       V7       V8       V9      V10 
0.000000 0.450000 1.116667 1.750000 2.433333 3.150000 3.800000 3.883333 5.233333 5.516667 
```

```r
# 3.
apply(dat[, seq(1, 40, by = 2)], 2, mean)
```

```{.output}
       V1        V3        V5        V7        V9       V11       V13       V15       V17       V19       V21       V23       V25       V27       V29       V31       V33       V35       V37       V39 
 0.000000  1.116667  2.433333  3.800000  5.233333  5.950000  8.350000  8.366667  9.583333 11.566667 13.250000 11.033333 10.000000  9.150000  7.333333  6.066667  5.116667  3.300000  2.483333  1.133333 
```

</div>

</div>

### Plotting

The mathematician Richard Hamming once said, "The purpose of computing is insight, not numbers," and the best way to develop insight is often to visualize data.
Visualization deserves an entire lecture (or course) of its own, but we can explore a few of R's plotting features.

Let's take a look at the average inflammation over time.
Recall that we already calculated these values above using `apply(dat, 2, mean)` and saved them in the variable `avg_day_inflammation`.
Plotting the values is done with the function `plot`.


```r
plot(avg_day_inflammation)
```

<img src="fig/01-starting-with-data-plot-avg-inflammation-1.png" title="plot of chunk plot-avg-inflammation" alt="plot of chunk plot-avg-inflammation" style="display: block; margin: auto;" />

Above, we gave the function `plot` a vector of numbers corresponding to the average inflammation per day across all patients.
`plot` created a scatter plot where the y-axis is the average inflammation level and the x-axis is the order, or index, of the values in the vector, which in this case correspond to the 40 days of treatment.
The result is roughly a linear rise and fall, which is suspicious: based on other studies, we expect a sharper rise and slower fall.
Let's have a look at two other statistics: the maximum and minimum inflammation per day.


```r
max_day_inflammation <- apply(dat, 2, max)
plot(max_day_inflammation)
```

<img src="fig/01-starting-with-data-plot-max-inflammation-1.png" title="plot of chunk plot-max-inflammation" alt="plot of chunk plot-max-inflammation" style="display: block; margin: auto;" />


```r
min_day_inflammation <- apply(dat, 2, min)
plot(min_day_inflammation)
```

<img src="fig/01-starting-with-data-plot-min-inflammation-1.png" title="plot of chunk plot-min-inflammation" alt="plot of chunk plot-min-inflammation" style="display: block; margin: auto;" />

The maximum value rises and falls perfectly smoothly, while the minimum seems to be a step function. Neither result seems particularly likely, so either there's a mistake in our calculations or something is wrong with our data.

<div class='challenge' markdown='1'>

## Plotting Data
Create a plot showing the standard deviation of the inflammation data for each day across all patients.

<div class='solution' markdown='1'>

## Solution
 

```r
sd_day_inflammation <- apply(dat, 2, sd)
plot(sd_day_inflammation)
```

<img src="fig/01-starting-with-data-dovetail-chunk-18-1-1.png" title="plot of chunk dovetail-chunk-18-1" alt="plot of chunk dovetail-chunk-18-1" style="display: block; margin: auto;" />

</div>

</div>

{% include links.md %}

<div class='keypoints' markdown='1'>

- Use `variable <- value` to assign a value to a variable in order to record it in memory.
- Objects are created on demand whenever a value is assigned to them.
- The function `dim` gives the dimensions of a data frame.
- Use `object[x, y]` to select a single element from a data frame.
- Use `from:to` to specify a sequence that includes the indices from `from` to `to`.
- All the indexing and subsetting that works on data frames also works on vectors.
- Use `#` to add comments to programs.
- Use `mean`, `max`, `min` and `sd` to calculate simple statistics.
- Use `apply` to calculate statistics across the rows or columns of a data frame.
- Use `plot` to create simple visualizations.

</div>


