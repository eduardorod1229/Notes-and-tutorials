How to use python in R
================
J. Eduardo Rodriguez Almaraz

 ### R markdown set up
 *Note: If r doesn't allow you to use reticulate you may need to specify the path were anaconda is installed*
```
{r}
knitr::opts_chunk$set(echo = TRUE)
library(reticulate)
```

## Importing libraries using R syntax

  - For this to work you need to download and install the `reticulate`
    library and either
    [miniconda](https://docs.conda.io/en/latest/miniconda.html) or
    [anaconda](https://www.anaconda.com) software With the following
    code you can import the python libraries as “R objects” which will
    appear in the **environment** section of R studio so for an RMarkdow
    you need to include the `'''{R}` syntax.
    
  - Additionally, you have to install the *python* libraries in
    *R* before using them you can do this by typing `py_install('name of
    library')` in the console. You only have to do this once. If this
    doesn’t work be sure to specify your path in the set up of the
    RMarkdown document


<!-- end list -->

``` r
sns<- import('seaborn')
plt<- import('matplotlib.pyplot')
pd <- import('pandas')
```

If the libraries are correctly imported then they will shouw up in the
*Global environment* pane as “Modules”

## Importing the dataset

Before manipulating anything we need a dataset, of course,In this case
we will use the system’s `AirPassengers` dataset. Note that we are using
R syntax to create this initial dataset and later we’ll go over on how
to interact with in in *Python*.

``` r
df <- datasets::AirPassengers
df1<-data.frame(tapply(df, list(year= floor(time(df)), month = month.abb[cycle(df)]),c))
df1<- df1[month.abb]
df1
```

    ##      Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec
    ## 1949 112 118 132 129 121 135 148 148 136 119 104 118
    ## 1950 115 126 141 135 125 149 170 170 158 133 114 140
    ## 1951 145 150 178 163 172 178 199 199 184 162 146 166
    ## 1952 171 180 193 181 183 218 230 242 209 191 172 194
    ## 1953 196 196 236 235 229 243 264 272 237 211 180 201
    ## 1954 204 188 235 227 234 264 302 293 259 229 203 229
    ## 1955 242 233 267 269 270 315 364 347 312 274 237 278
    ## 1956 284 277 317 313 318 374 413 405 355 306 271 306
    ## 1957 315 301 356 348 355 422 465 467 404 347 305 336
    ## 1958 340 318 362 348 363 435 491 505 404 359 310 337
    ## 1959 360 342 406 396 420 472 548 559 463 407 362 405
    ## 1960 417 391 419 461 472 535 622 606 508 461 390 432

Now we have the data set you can explore it in your *global environment*
window or by using the `df1` command in the console or script.

## Creating the plots

Following, you can pull in from R your data frame into python using the
syntax below. Note that for every R object (variable, list, tupple, data
frame) you need to let python know that is available in R so it finds
it. Otherwise you will run an error that the object has not been
assigned.

I choose to do this method given that R markdown will create the figures
and you will be able to visualize them but you wouldn’t be able to see
them in the final document if python syntax is used WITHIN R Hence, the
chunk below begins with {python} to specify that we will be using python
syntax. You can include R code in the document as follows:

``` python
import pandas as pd
df1 = pd.DataFrame(r.df1)
print(df1)
```

    ##         Jan    Feb    Mar    Apr    May  ...    Aug    Sep    Oct    Nov    Dec
    ## 1949  112.0  118.0  132.0  129.0  121.0  ...  148.0  136.0  119.0  104.0  118.0
    ## 1950  115.0  126.0  141.0  135.0  125.0  ...  170.0  158.0  133.0  114.0  140.0
    ## 1951  145.0  150.0  178.0  163.0  172.0  ...  199.0  184.0  162.0  146.0  166.0
    ## 1952  171.0  180.0  193.0  181.0  183.0  ...  242.0  209.0  191.0  172.0  194.0
    ## 1953  196.0  196.0  236.0  235.0  229.0  ...  272.0  237.0  211.0  180.0  201.0
    ## 1954  204.0  188.0  235.0  227.0  234.0  ...  293.0  259.0  229.0  203.0  229.0
    ## 1955  242.0  233.0  267.0  269.0  270.0  ...  347.0  312.0  274.0  237.0  278.0
    ## 1956  284.0  277.0  317.0  313.0  318.0  ...  405.0  355.0  306.0  271.0  306.0
    ## 1957  315.0  301.0  356.0  348.0  355.0  ...  467.0  404.0  347.0  305.0  336.0
    ## 1958  340.0  318.0  362.0  348.0  363.0  ...  505.0  404.0  359.0  310.0  337.0
    ## 1959  360.0  342.0  406.0  396.0  420.0  ...  559.0  463.0  407.0  362.0  405.0
    ## 1960  417.0  391.0  419.0  461.0  472.0  ...  606.0  508.0  461.0  390.0  432.0
    ## 
    ## [12 rows x 12 columns]

The print command is just to show how we are really importing the same
data into python. We first need to import the libraries. I am unsure if
each library needs its own line but it seems that R handles that better
than a whole chunk with all the libraries….Unfortunate

``` python
import matplotlib.pyplot as plt
```

Lets build the plot using python syntax

``` python
import seaborn as sns
sns.heatmap(df1, fmt='g', cmap='viridis')
plt.show()
```

![](How-to-use-python-in-R_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

**Success\!**

Lets create
pairplots

``` python
sns.pairplot(r.iris, hue = 'Species')
```

    ## <seaborn.axisgrid.PairGrid object at 0x7fb3743ee2e8>

``` python
plt.show()
```

![](How-to-use-python-in-R_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->


## Alternative method to run python within R 
Up to importing the dataset will be the same. To create the plots in python.

  - Begin by telling R markdown that you will be using R syntax `{r}`

<!-- end list -->

``` r
sns$heatmap(r_to_py(df1), fmt='g', cmap='viridis')
```

    ## AxesSubplot(0.0622357,0.0602222;0.650798x0.921649)

``` r
plt$show()
```

Note that using `r_to_py` has the same effect than using `r.df1`. Thi is just the
way we have to tell python that we will be using an `{r}` "object".

``` r
sns$pairplot(r_to_py(iris), hue='Species')
```

    ## <seaborn.axisgrid.PairGrid>

``` r
plt$show()
```

That’s it\! You have now two options to interact with R and python. I
still prefer googlecollab but this works fine as well.


Be sure to read the reticulate documentation. This tutorial was done
following instructions from [towards data
science](https://towardsdatascience.com/python-seaborn-plots-in-r-using-reticulate-fb59cebf61a7).
