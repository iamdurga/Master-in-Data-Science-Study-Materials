#ggplot2

## Grammer :

A grammar provides a foundation for understanding a diffrent type of
graphics. A grammar may also help guide us on what a well-formed or
correct graphic looks like, but there will still be many grammatically
correct but nonsensical graphics. This is easy to see by analogy to the
English language: good grammar is just the first step in creating a good
sentence.

## Grammar of graphics:

A grammar of graphics is a tool that from which it enables us to clearly
describe the components of a graphic. Such a grammar allows us to move
beyond named graphics (e.g., the “scatterplot”) and gain insight into
the deep structure that underlies statistical graphics. • ggplot2
proposes an alternative parameterization of the grammar, based around
the idea of building up a graphic from multiple layers of data.

## Components of ggplot2:

-   Data and aesthetic mapping +
-   Geometric objects
-   Scale
-   Facet Specification
-   Statistical Transformation
-   Coordinate Syatem

## Layered grammar of graphics:

Together, the data, mappings,statistical transformation, and geometric
object form a layer. Plot may have different layer.Layers are
responsible for creating the objects that we expected on the plot.

## How to use ggplot2 in R.

For this we should have to installed ggplot2 package in our IDE. Let us
use ggplot in R builtin data `diamonds`.

``` r
library(ggplot2)
ggplot(diamonds, aes(carat,price)) + geom_point()
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-1-1.png)
geom_point is used for scatter plot. From above figure we can see that
as diamond’s carat increases prices is also increases. We can not see
how the data distibuted for this let make some changes in our code.

``` r
ggplot(diamonds,aes(carat,price)) + geom_point() +
  scale_x_continuous() + scale_y_continuous()
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-2-1.png) We
can see better distribution of points than previous plot. We clearly see
that caret and price variable are not linearly distributed. To make it
linealy distributable also make some changes in our code.

``` r
ggplot(diamonds, aes(carat,price)) + geom_point() +
  stat_smooth(method = lm) + scale_x_log10() + scale_y_log10()
```

    ## `geom_smooth()` using formula 'y ~ x'

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-3-1.png)
From above graph relationship between price and carat variables is
linear. If we try the code without `stat_smooth(method= lm)` we can not
see linear line in graph.

``` r
ggplot(diamonds, aes(carat,price)) + geom_point() +
  scale_x_log10() + scale_y_log10()
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-4-1.png) ##
Lets make histogram of `diamonds` data.

``` r
ggplot(diamonds, aes(price)) + geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-5-1.png) To
build histogram we use function `geom_histogram()`. We should note
histogram is made on one dimensional data. If we want to add title of
the plot we can do,

``` r
ggplot(diamonds, aes(price)) + geom_histogram() + ggtitle("ggplot2 Histogram")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-6-1.png) ##
Let us try some others ggplot2 features in R builtin data `mtcars`.

``` r
ggplot(mpg, aes(x = displ, y = hwy)) +
geom_point()
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-7-1.png) The
figure above scatter plot between
’hwy`and`displ`variables of mtcars data from figure we can see as the values of`hwy`increase vales of`displ\`
variables slightly decreases.

## let’s Add geam_smooth() what will happen?

``` r
ggplot(mpg, aes(displ, hwy)) + geom_point() + geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-8-1.png) We
see there is smooth line appear on the middle of the data points. ##
Adding “wiggliness” in the smoothing plot:

``` r
ggplot(mpg, aes(displ, hwy)) + geom_point() + geom_smooth(span = 0.2) 
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-9-1.png)
What changes come in the above graph and previous graph. Let us again
check by keeping sapn = 1.

``` r
ggplot(mpg, aes(displ, hwy)) + geom_point()+ geom_smooth(span = 1)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-10-1.png) We
can make sence that by default ggplot kept value of span 1. If we set
method= lm inside geom_smooth() we can find stright smooth line. Let us
try.

``` r
ggplot(mpg, aes(displ, hwy)) + geom_point() + geom_smooth(method = lm) 
```

    ## `geom_smooth()` using formula 'y ~ x'

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-11-1.png) ##
Let Modify our code little,

``` r
ggplot(mpg, aes(displ, hwy)) + geom_point() + geom_smooth(method = lm, se= FALSE) 
```

    ## `geom_smooth()` using formula 'y ~ x'

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-12-1.png) By
`se= FALSE` we add smooth line.

## Fixed color

``` r
ggplot(mpg, aes(displ,hwy)) + geom_point(color = 'red')
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-13-1.png) ##
Changing color by variable attributes:

``` r
ggplot(mpg, aes(displ, hwy, colour = class)) + 
geom_point()
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-14-1.png)
Here we give colors according to variables name.

## Getting multiple scatterplot of attributes:

We get multiple scatterplot by using `facet_wrap()` function.

``` r
ggplot(mpg, aes(displ, hwy)) + geom_point() + 
facet_wrap(~class)
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-15-1.png) In
above figure we find distribution of various variables along with displ
and hwy variables.

## Histogram

``` r
ggplot(mpg, aes(hwy)) + geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-16-1.png)
hwy variable bin automatically.

## Changing bin size of the histogram:

``` r
ggplot(mpg, aes(hwy)) + geom_histogram(binwidth = 2.5)
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-17-1.png) ##
Frequency polygon: A frequency polygon is a line graph of class
frequency plotted against class midpoint. It can be obtained by joining
the midpoints of the tops of the rectangles in the histogram.

``` r
ggplot(mpg, aes(hwy)) + geom_freqpoly()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-18-1.png) ##
Change Bin size of frequency Polygon

``` r
ggplot(mpg, aes(hwy)) + geom_freqpoly(binwidth= 1)
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-19-1.png) We
can see the effect of binwidth from figure by comparing above figure
with previous one.

## Histogram with faceting:

We already discuss about what a facet do in scatter plot. Similarly in
histogram it gives multiple subplots.

``` r
ggplot(mpg, aes(displ, fill = drv)) + geom_histogram(binwidth = 0.5) + 
facet_wrap(~drv, ncol = 1)
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-20-1.png) ##
Bar plot

``` r
ggplot(mpg, aes(manufacturer)) + geom_bar()
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-21-1.png) We
can draw bar plot in `geom_bar()` function from bar plot we can see
`dodge` and `toyotao` has maximun frequency.

## Let’s Use alpha inside geom\_ point()

``` r
ggplot(mpg, aes(cty, hwy)) + geom_point(alpha = 1 / 3)
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-22-1.png)
Alpha refers to the opacity of a geom. Values of alpha range from 0 to
1, with lower values corresponding to more transparent colors.

## Mofifying the axes

``` r
ggplot(mpg, aes(cty, hwy)) +geom_point(alpha = 1 / 3) + xlab("city driving (mpg)") + 
ylab("highway driving (mpg)")
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-23-1.png)

``` r
ggplot(mpg, aes(cty, hwy)) + geom_point(alpha = 1 / 3) + xlab(NULL) + 
 ylab(NULL)
```

![](ggplot2-blog_files/figure-markdown_github/unnamed-chunk-24-1.png)
