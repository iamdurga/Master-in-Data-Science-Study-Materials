``` r
library(magrittr)
```

# Data Analysis Using Pipe Operator in R.

## What is pipe oprator:

Pipe operator are a very powerful tool for clearly expresing a sequence
of multiple operations. Pipe come from `magrittr` package. Pipe helps us
to write code in a way that is easier to read write and understand.

## Why to use Pipe:

R is functional language it requeres lots of parenthesis while doing
complex programming, this often means we have to nest those parenthesis
together for statistical computing. This makes R codes hard to read and
understand. In this suitation pipe help us. There are three types of
pipe operators. They are,

### %\< \>% Compound Operator:

Updates values for same variable.

``` r
x <- rnorm(100)
x %<>% abs %>% sort
x
```

    ##   [1] 0.009983937 0.016608923 0.023887211 0.028489290 0.058244965 0.077193140
    ##   [7] 0.112791553 0.120680475 0.142512597 0.146191765 0.147149809 0.165798798
    ##  [13] 0.169114223 0.178767244 0.186996059 0.215662105 0.225165303 0.225711892
    ##  [19] 0.234813083 0.255162793 0.257619673 0.263657947 0.274256886 0.278533752
    ##  [25] 0.294561592 0.301091692 0.303962890 0.341956524 0.344785272 0.356188353
    ##  [31] 0.364966970 0.386465486 0.392640172 0.397031027 0.399785232 0.401835250
    ##  [37] 0.454831319 0.462229304 0.480710234 0.483437251 0.483892691 0.486781500
    ##  [43] 0.495098266 0.500368634 0.552609131 0.557855069 0.562689263 0.569710993
    ##  [49] 0.570507316 0.579221386 0.645926049 0.647673949 0.651625795 0.669647688
    ##  [55] 0.685098831 0.691043660 0.695729376 0.718395762 0.735014583 0.806662050
    ##  [61] 0.829931993 0.864247899 0.872475691 0.885884373 0.894854852 0.905258561
    ##  [67] 0.913041727 0.956071568 1.034663289 1.037072257 1.046854227 1.123876928
    ##  [73] 1.155893261 1.160593930 1.170788411 1.182924108 1.202387282 1.273220874
    ##  [79] 1.273903761 1.306092878 1.322683135 1.338790649 1.347844508 1.351232284
    ##  [85] 1.361332556 1.389807581 1.392778554 1.421035715 1.444100750 1.480067832
    ##  [91] 1.603077777 1.618609928 1.633015485 1.682547525 1.703995370 1.709338446
    ##  [97] 1.750947764 1.817871496 2.289798927 2.358908977

Here at first %\<\>% find absolute values of x and and then it sort in
uncreasing order.

### %>% tee Operator:

Normally, code ends after plot command but the “tee” pipe operator
allows it to continue for the next arguement

``` r
set.seed(123) 
rnorm(200) %>% 
matrix(ncol = 2) %T>% 
plot %>% 
colSums
```

![](Pipe-blog_files/figure-markdown_github/unnamed-chunk-3-1.png)

    ## [1]   9.040591 -10.754680

### %$%

This operator is useful when we want to pipe a dataframe which may
contains many columns in to a function that is applied to some of the
columns.

``` r
data.frame(z = rnorm(100)) %$%
ts.plot(z)
```

![](Pipe-blog_files/figure-markdown_github/unnamed-chunk-4-1.png)

## let’s do some Data Analysis in R’s Builtin data iris with Pipe Operator:

First, let’s us check what attributes `iris` data contains.

``` r
head(iris)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa

`iris` data have four columns they are
`Sepal.Length,Sepal.Width, Petal.Length ,Petal.Width Species`.

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
iris %>%
  select(contains('Sepal')) %>%
  mutate(Sepal.Area=Sepal.Length * Sepal.Width)
```

    ##     Sepal.Length Sepal.Width Sepal.Area
    ## 1            5.1         3.5      17.85
    ## 2            4.9         3.0      14.70
    ## 3            4.7         3.2      15.04
    ## 4            4.6         3.1      14.26
    ## 5            5.0         3.6      18.00
    ## 6            5.4         3.9      21.06
    ## 7            4.6         3.4      15.64
    ## 8            5.0         3.4      17.00
    ## 9            4.4         2.9      12.76
    ## 10           4.9         3.1      15.19
    ## 11           5.4         3.7      19.98
    ## 12           4.8         3.4      16.32
    ## 13           4.8         3.0      14.40
    ## 14           4.3         3.0      12.90
    ## 15           5.8         4.0      23.20
    ## 16           5.7         4.4      25.08
    ## 17           5.4         3.9      21.06
    ## 18           5.1         3.5      17.85
    ## 19           5.7         3.8      21.66
    ## 20           5.1         3.8      19.38
    ## 21           5.4         3.4      18.36
    ## 22           5.1         3.7      18.87
    ## 23           4.6         3.6      16.56
    ## 24           5.1         3.3      16.83
    ## 25           4.8         3.4      16.32
    ## 26           5.0         3.0      15.00
    ## 27           5.0         3.4      17.00
    ## 28           5.2         3.5      18.20
    ## 29           5.2         3.4      17.68
    ## 30           4.7         3.2      15.04
    ## 31           4.8         3.1      14.88
    ## 32           5.4         3.4      18.36
    ## 33           5.2         4.1      21.32
    ## 34           5.5         4.2      23.10
    ## 35           4.9         3.1      15.19
    ## 36           5.0         3.2      16.00
    ## 37           5.5         3.5      19.25
    ## 38           4.9         3.6      17.64
    ## 39           4.4         3.0      13.20
    ## 40           5.1         3.4      17.34
    ## 41           5.0         3.5      17.50
    ## 42           4.5         2.3      10.35
    ## 43           4.4         3.2      14.08
    ## 44           5.0         3.5      17.50
    ## 45           5.1         3.8      19.38
    ## 46           4.8         3.0      14.40
    ## 47           5.1         3.8      19.38
    ## 48           4.6         3.2      14.72
    ## 49           5.3         3.7      19.61
    ## 50           5.0         3.3      16.50
    ## 51           7.0         3.2      22.40
    ## 52           6.4         3.2      20.48
    ## 53           6.9         3.1      21.39
    ## 54           5.5         2.3      12.65
    ## 55           6.5         2.8      18.20
    ## 56           5.7         2.8      15.96
    ## 57           6.3         3.3      20.79
    ## 58           4.9         2.4      11.76
    ## 59           6.6         2.9      19.14
    ## 60           5.2         2.7      14.04
    ## 61           5.0         2.0      10.00
    ## 62           5.9         3.0      17.70
    ## 63           6.0         2.2      13.20
    ## 64           6.1         2.9      17.69
    ## 65           5.6         2.9      16.24
    ## 66           6.7         3.1      20.77
    ## 67           5.6         3.0      16.80
    ## 68           5.8         2.7      15.66
    ## 69           6.2         2.2      13.64
    ## 70           5.6         2.5      14.00
    ## 71           5.9         3.2      18.88
    ## 72           6.1         2.8      17.08
    ## 73           6.3         2.5      15.75
    ## 74           6.1         2.8      17.08
    ## 75           6.4         2.9      18.56
    ## 76           6.6         3.0      19.80
    ## 77           6.8         2.8      19.04
    ## 78           6.7         3.0      20.10
    ## 79           6.0         2.9      17.40
    ## 80           5.7         2.6      14.82
    ## 81           5.5         2.4      13.20
    ## 82           5.5         2.4      13.20
    ## 83           5.8         2.7      15.66
    ## 84           6.0         2.7      16.20
    ## 85           5.4         3.0      16.20
    ## 86           6.0         3.4      20.40
    ## 87           6.7         3.1      20.77
    ## 88           6.3         2.3      14.49
    ## 89           5.6         3.0      16.80
    ## 90           5.5         2.5      13.75
    ## 91           5.5         2.6      14.30
    ## 92           6.1         3.0      18.30
    ## 93           5.8         2.6      15.08
    ## 94           5.0         2.3      11.50
    ## 95           5.6         2.7      15.12
    ## 96           5.7         3.0      17.10
    ## 97           5.7         2.9      16.53
    ## 98           6.2         2.9      17.98
    ## 99           5.1         2.5      12.75
    ## 100          5.7         2.8      15.96
    ## 101          6.3         3.3      20.79
    ## 102          5.8         2.7      15.66
    ## 103          7.1         3.0      21.30
    ## 104          6.3         2.9      18.27
    ## 105          6.5         3.0      19.50
    ## 106          7.6         3.0      22.80
    ## 107          4.9         2.5      12.25
    ## 108          7.3         2.9      21.17
    ## 109          6.7         2.5      16.75
    ## 110          7.2         3.6      25.92
    ## 111          6.5         3.2      20.80
    ## 112          6.4         2.7      17.28
    ## 113          6.8         3.0      20.40
    ## 114          5.7         2.5      14.25
    ## 115          5.8         2.8      16.24
    ## 116          6.4         3.2      20.48
    ## 117          6.5         3.0      19.50
    ## 118          7.7         3.8      29.26
    ## 119          7.7         2.6      20.02
    ## 120          6.0         2.2      13.20
    ## 121          6.9         3.2      22.08
    ## 122          5.6         2.8      15.68
    ## 123          7.7         2.8      21.56
    ## 124          6.3         2.7      17.01
    ## 125          6.7         3.3      22.11
    ## 126          7.2         3.2      23.04
    ## 127          6.2         2.8      17.36
    ## 128          6.1         3.0      18.30
    ## 129          6.4         2.8      17.92
    ## 130          7.2         3.0      21.60
    ## 131          7.4         2.8      20.72
    ## 132          7.9         3.8      30.02
    ## 133          6.4         2.8      17.92
    ## 134          6.3         2.8      17.64
    ## 135          6.1         2.6      15.86
    ## 136          7.7         3.0      23.10
    ## 137          6.3         3.4      21.42
    ## 138          6.4         3.1      19.84
    ## 139          6.0         3.0      18.00
    ## 140          6.9         3.1      21.39
    ## 141          6.7         3.1      20.77
    ## 142          6.9         3.1      21.39
    ## 143          5.8         2.7      15.66
    ## 144          6.8         3.2      21.76
    ## 145          6.7         3.3      22.11
    ## 146          6.7         3.0      20.10
    ## 147          6.3         2.5      15.75
    ## 148          6.5         3.0      19.50
    ## 149          6.2         3.4      21.08
    ## 150          5.9         3.0      17.70

`contains` function only select those columns which contains **sepal**
word. Here we also import `library(dplyr)` dplyr packages is mainly used
for data cleaning purpose. It contaims five buitin function they are:

-   select

-   filter

-   arrange

-   mutate

These five functions can do majority of data manipulatlation for data
analysis task.

In above code mutate function added new column (“Sepal.Area”) in the
data frame.

``` r
iris %>%
  select(starts_with('Sepal')) %>%
  filter(Sepal.Length >= 5.0) %>%
  arrange(Sepal.Length)
```

    ##     Sepal.Length Sepal.Width
    ## 1            5.0         3.6
    ## 2            5.0         3.4
    ## 3            5.0         3.0
    ## 4            5.0         3.4
    ## 5            5.0         3.2
    ## 6            5.0         3.5
    ## 7            5.0         3.5
    ## 8            5.0         3.3
    ## 9            5.0         2.0
    ## 10           5.0         2.3
    ## 11           5.1         3.5
    ## 12           5.1         3.5
    ## 13           5.1         3.8
    ## 14           5.1         3.7
    ## 15           5.1         3.3
    ## 16           5.1         3.4
    ## 17           5.1         3.8
    ## 18           5.1         3.8
    ## 19           5.1         2.5
    ## 20           5.2         3.5
    ## 21           5.2         3.4
    ## 22           5.2         4.1
    ## 23           5.2         2.7
    ## 24           5.3         3.7
    ## 25           5.4         3.9
    ## 26           5.4         3.7
    ## 27           5.4         3.9
    ## 28           5.4         3.4
    ## 29           5.4         3.4
    ## 30           5.4         3.0
    ## 31           5.5         4.2
    ## 32           5.5         3.5
    ## 33           5.5         2.3
    ## 34           5.5         2.4
    ## 35           5.5         2.4
    ## 36           5.5         2.5
    ## 37           5.5         2.6
    ## 38           5.6         2.9
    ## 39           5.6         3.0
    ## 40           5.6         2.5
    ## 41           5.6         3.0
    ## 42           5.6         2.7
    ## 43           5.6         2.8
    ## 44           5.7         4.4
    ## 45           5.7         3.8
    ## 46           5.7         2.8
    ## 47           5.7         2.6
    ## 48           5.7         3.0
    ## 49           5.7         2.9
    ## 50           5.7         2.8
    ## 51           5.7         2.5
    ## 52           5.8         4.0
    ## 53           5.8         2.7
    ## 54           5.8         2.7
    ## 55           5.8         2.6
    ## 56           5.8         2.7
    ## 57           5.8         2.8
    ## 58           5.8         2.7
    ## 59           5.9         3.0
    ## 60           5.9         3.2
    ## 61           5.9         3.0
    ## 62           6.0         2.2
    ## 63           6.0         2.9
    ## 64           6.0         2.7
    ## 65           6.0         3.4
    ## 66           6.0         2.2
    ## 67           6.0         3.0
    ## 68           6.1         2.9
    ## 69           6.1         2.8
    ## 70           6.1         2.8
    ## 71           6.1         3.0
    ## 72           6.1         3.0
    ## 73           6.1         2.6
    ## 74           6.2         2.2
    ## 75           6.2         2.9
    ## 76           6.2         2.8
    ## 77           6.2         3.4
    ## 78           6.3         3.3
    ## 79           6.3         2.5
    ## 80           6.3         2.3
    ## 81           6.3         3.3
    ## 82           6.3         2.9
    ## 83           6.3         2.7
    ## 84           6.3         2.8
    ## 85           6.3         3.4
    ## 86           6.3         2.5
    ## 87           6.4         3.2
    ## 88           6.4         2.9
    ## 89           6.4         2.7
    ## 90           6.4         3.2
    ## 91           6.4         2.8
    ## 92           6.4         2.8
    ## 93           6.4         3.1
    ## 94           6.5         2.8
    ## 95           6.5         3.0
    ## 96           6.5         3.2
    ## 97           6.5         3.0
    ## 98           6.5         3.0
    ## 99           6.6         2.9
    ## 100          6.6         3.0
    ## 101          6.7         3.1
    ## 102          6.7         3.0
    ## 103          6.7         3.1
    ## 104          6.7         2.5
    ## 105          6.7         3.3
    ## 106          6.7         3.1
    ## 107          6.7         3.3
    ## 108          6.7         3.0
    ## 109          6.8         2.8
    ## 110          6.8         3.0
    ## 111          6.8         3.2
    ## 112          6.9         3.1
    ## 113          6.9         3.2
    ## 114          6.9         3.1
    ## 115          6.9         3.1
    ## 116          7.0         3.2
    ## 117          7.1         3.0
    ## 118          7.2         3.6
    ## 119          7.2         3.2
    ## 120          7.2         3.0
    ## 121          7.3         2.9
    ## 122          7.4         2.8
    ## 123          7.6         3.0
    ## 124          7.7         3.8
    ## 125          7.7         2.6
    ## 126          7.7         2.8
    ## 127          7.7         3.0
    ## 128          7.9         3.8

By using above code we want to select only those columns which has
**Sepal** as prefix and want to filter those values which have values
greater than `Sepal.Length >= 5.0` and finally we did arrange in
accending order. If we to sort in decending order we simply need to type
**desc** inside arrange function as like below.

``` r
iris %>%
  select(starts_with('Sepal')) %>%
  filter(Sepal.Length >= 5.0) %>%
  arrange(desc(Sepal.Length))
```

    ##     Sepal.Length Sepal.Width
    ## 1            7.9         3.8
    ## 2            7.7         3.8
    ## 3            7.7         2.6
    ## 4            7.7         2.8
    ## 5            7.7         3.0
    ## 6            7.6         3.0
    ## 7            7.4         2.8
    ## 8            7.3         2.9
    ## 9            7.2         3.6
    ## 10           7.2         3.2
    ## 11           7.2         3.0
    ## 12           7.1         3.0
    ## 13           7.0         3.2
    ## 14           6.9         3.1
    ## 15           6.9         3.2
    ## 16           6.9         3.1
    ## 17           6.9         3.1
    ## 18           6.8         2.8
    ## 19           6.8         3.0
    ## 20           6.8         3.2
    ## 21           6.7         3.1
    ## 22           6.7         3.0
    ## 23           6.7         3.1
    ## 24           6.7         2.5
    ## 25           6.7         3.3
    ## 26           6.7         3.1
    ## 27           6.7         3.3
    ## 28           6.7         3.0
    ## 29           6.6         2.9
    ## 30           6.6         3.0
    ## 31           6.5         2.8
    ## 32           6.5         3.0
    ## 33           6.5         3.2
    ## 34           6.5         3.0
    ## 35           6.5         3.0
    ## 36           6.4         3.2
    ## 37           6.4         2.9
    ## 38           6.4         2.7
    ## 39           6.4         3.2
    ## 40           6.4         2.8
    ## 41           6.4         2.8
    ## 42           6.4         3.1
    ## 43           6.3         3.3
    ## 44           6.3         2.5
    ## 45           6.3         2.3
    ## 46           6.3         3.3
    ## 47           6.3         2.9
    ## 48           6.3         2.7
    ## 49           6.3         2.8
    ## 50           6.3         3.4
    ## 51           6.3         2.5
    ## 52           6.2         2.2
    ## 53           6.2         2.9
    ## 54           6.2         2.8
    ## 55           6.2         3.4
    ## 56           6.1         2.9
    ## 57           6.1         2.8
    ## 58           6.1         2.8
    ## 59           6.1         3.0
    ## 60           6.1         3.0
    ## 61           6.1         2.6
    ## 62           6.0         2.2
    ## 63           6.0         2.9
    ## 64           6.0         2.7
    ## 65           6.0         3.4
    ## 66           6.0         2.2
    ## 67           6.0         3.0
    ## 68           5.9         3.0
    ## 69           5.9         3.2
    ## 70           5.9         3.0
    ## 71           5.8         4.0
    ## 72           5.8         2.7
    ## 73           5.8         2.7
    ## 74           5.8         2.6
    ## 75           5.8         2.7
    ## 76           5.8         2.8
    ## 77           5.8         2.7
    ## 78           5.7         4.4
    ## 79           5.7         3.8
    ## 80           5.7         2.8
    ## 81           5.7         2.6
    ## 82           5.7         3.0
    ## 83           5.7         2.9
    ## 84           5.7         2.8
    ## 85           5.7         2.5
    ## 86           5.6         2.9
    ## 87           5.6         3.0
    ## 88           5.6         2.5
    ## 89           5.6         3.0
    ## 90           5.6         2.7
    ## 91           5.6         2.8
    ## 92           5.5         4.2
    ## 93           5.5         3.5
    ## 94           5.5         2.3
    ## 95           5.5         2.4
    ## 96           5.5         2.4
    ## 97           5.5         2.5
    ## 98           5.5         2.6
    ## 99           5.4         3.9
    ## 100          5.4         3.7
    ## 101          5.4         3.9
    ## 102          5.4         3.4
    ## 103          5.4         3.4
    ## 104          5.4         3.0
    ## 105          5.3         3.7
    ## 106          5.2         3.5
    ## 107          5.2         3.4
    ## 108          5.2         4.1
    ## 109          5.2         2.7
    ## 110          5.1         3.5
    ## 111          5.1         3.5
    ## 112          5.1         3.8
    ## 113          5.1         3.7
    ## 114          5.1         3.3
    ## 115          5.1         3.4
    ## 116          5.1         3.8
    ## 117          5.1         3.8
    ## 118          5.1         2.5
    ## 119          5.0         3.6
    ## 120          5.0         3.4
    ## 121          5.0         3.0
    ## 122          5.0         3.4
    ## 123          5.0         3.2
    ## 124          5.0         3.5
    ## 125          5.0         3.5
    ## 126          5.0         3.3
    ## 127          5.0         2.0
    ## 128          5.0         2.3

## Let’s try one Different Step:

Let us observer what will come in result if we use `end_with` in place
of `starts_with`.

``` r
iris %>%
  select(ends_with('Length')) %>%
  rowwise() %>%
  mutate(Length.Diff = Sepal.Length- Petal.Length)
```

    ## # A tibble: 150 x 3
    ## # Rowwise: 
    ##    Sepal.Length Petal.Length Length.Diff
    ##           <dbl>        <dbl>       <dbl>
    ##  1          5.1          1.4         3.7
    ##  2          4.9          1.4         3.5
    ##  3          4.7          1.3         3.4
    ##  4          4.6          1.5         3.1
    ##  5          5            1.4         3.6
    ##  6          5.4          1.7         3.7
    ##  7          4.6          1.4         3.2
    ##  8          5            1.5         3.5
    ##  9          4.4          1.4         3  
    ## 10          4.9          1.5         3.4
    ## # ... with 140 more rows

I hope we have observed what new came here. Here, we got prefix as
**Length**. It means we got only those columns which has length as it’s
prefix. Also by using mutate functiin we created new column
**Length.Diff**.

## Use `transmute` function

``` r
iris %>%
  select(contains('Sepal'), Species) %>%
  transmute(Sepal.Area= Sepal.Length * Sepal.Width)
```

    ##     Sepal.Area
    ## 1        17.85
    ## 2        14.70
    ## 3        15.04
    ## 4        14.26
    ## 5        18.00
    ## 6        21.06
    ## 7        15.64
    ## 8        17.00
    ## 9        12.76
    ## 10       15.19
    ## 11       19.98
    ## 12       16.32
    ## 13       14.40
    ## 14       12.90
    ## 15       23.20
    ## 16       25.08
    ## 17       21.06
    ## 18       17.85
    ## 19       21.66
    ## 20       19.38
    ## 21       18.36
    ## 22       18.87
    ## 23       16.56
    ## 24       16.83
    ## 25       16.32
    ## 26       15.00
    ## 27       17.00
    ## 28       18.20
    ## 29       17.68
    ## 30       15.04
    ## 31       14.88
    ## 32       18.36
    ## 33       21.32
    ## 34       23.10
    ## 35       15.19
    ## 36       16.00
    ## 37       19.25
    ## 38       17.64
    ## 39       13.20
    ## 40       17.34
    ## 41       17.50
    ## 42       10.35
    ## 43       14.08
    ## 44       17.50
    ## 45       19.38
    ## 46       14.40
    ## 47       19.38
    ## 48       14.72
    ## 49       19.61
    ## 50       16.50
    ## 51       22.40
    ## 52       20.48
    ## 53       21.39
    ## 54       12.65
    ## 55       18.20
    ## 56       15.96
    ## 57       20.79
    ## 58       11.76
    ## 59       19.14
    ## 60       14.04
    ## 61       10.00
    ## 62       17.70
    ## 63       13.20
    ## 64       17.69
    ## 65       16.24
    ## 66       20.77
    ## 67       16.80
    ## 68       15.66
    ## 69       13.64
    ## 70       14.00
    ## 71       18.88
    ## 72       17.08
    ## 73       15.75
    ## 74       17.08
    ## 75       18.56
    ## 76       19.80
    ## 77       19.04
    ## 78       20.10
    ## 79       17.40
    ## 80       14.82
    ## 81       13.20
    ## 82       13.20
    ## 83       15.66
    ## 84       16.20
    ## 85       16.20
    ## 86       20.40
    ## 87       20.77
    ## 88       14.49
    ## 89       16.80
    ## 90       13.75
    ## 91       14.30
    ## 92       18.30
    ## 93       15.08
    ## 94       11.50
    ## 95       15.12
    ## 96       17.10
    ## 97       16.53
    ## 98       17.98
    ## 99       12.75
    ## 100      15.96
    ## 101      20.79
    ## 102      15.66
    ## 103      21.30
    ## 104      18.27
    ## 105      19.50
    ## 106      22.80
    ## 107      12.25
    ## 108      21.17
    ## 109      16.75
    ## 110      25.92
    ## 111      20.80
    ## 112      17.28
    ## 113      20.40
    ## 114      14.25
    ## 115      16.24
    ## 116      20.48
    ## 117      19.50
    ## 118      29.26
    ## 119      20.02
    ## 120      13.20
    ## 121      22.08
    ## 122      15.68
    ## 123      21.56
    ## 124      17.01
    ## 125      22.11
    ## 126      23.04
    ## 127      17.36
    ## 128      18.30
    ## 129      17.92
    ## 130      21.60
    ## 131      20.72
    ## 132      30.02
    ## 133      17.92
    ## 134      17.64
    ## 135      15.86
    ## 136      23.10
    ## 137      21.42
    ## 138      19.84
    ## 139      18.00
    ## 140      21.39
    ## 141      20.77
    ## 142      21.39
    ## 143      15.66
    ## 144      21.76
    ## 145      22.11
    ## 146      20.10
    ## 147      15.75
    ## 148      19.50
    ## 149      21.08
    ## 150      17.70

Here **transmute** funtion only show new columns it hide Sepal.Length
and Sepal.Width columns.

## What a difference we observe when we use pipe operator and genral R function:

Let us clearify it from code

### Using R general function

Assign value of x with given column vector

``` r
x <- c(0.109, 0.359, 0.63, 0.996, 0.515, 0.142, 0.017, 0.829, 0.907)
```

Here we want to compute the following with in single line of code,

-   Compute the logarithm of `x`,

-   return suitably lagged and iterated difference

-   compute the exponential function and round the result

``` r
round(exp(diff(log(x))), 1)
```

    ## [1]  3.3  1.8  1.6  0.5  0.3  0.1 48.8  1.1

Let’s to some code using pipe operators.

``` r
x %>%log() %>%
  diff() %>%
  exp()%>%
  round(1)
```

    ## [1]  3.3  1.8  1.6  0.5  0.3  0.1 48.8  1.1

In above pipe come handly because we did not need to nest with lots of
parenthesis. Finally we got the same result.

However, there are lots of advantages of pipe operator there are some
creteria to use pipe operator. Following are the some of the limitations
of pipe operators.

-   Our pipes are longer than (say) ten steps

-   We have multiple inputs or outputs

-   We are starting to think about a directed graph with a complex
    dependency structure

-   We’re doing internal package development

In above mention cases we do not prefer to use pipe operator.
