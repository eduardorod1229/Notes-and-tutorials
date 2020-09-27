
## How to use python in R

#### J. Eduardo Rodriguez Almaraz


* To see the full explanation and datasets [click here](https://github.com/eduardorod1229/Notes-and-tutorials/blob/master/How-to-use-python-in-R_files/How-to-use-python-in-R.md)
* You also can see the R Markdown that used to create this tutorial and try it out yourself [here](https://github.com/eduardorod1229/Notes-and-tutorials/blob/master/How-to-use-python-in-R.Rmd)

## Importing libraries using R syntax

* For this to work you need to download and install the `reticulate` library using `install.packages('reticulate')` and either [miniconda](https://docs.conda.io/en/latest/miniconda.html) or [anaconda](https://www.anaconda.com) software
With the following code you can import the python libraries as "R objects" which will appear in the **environment** section of R studio so for an RMarkdown you need to include the `'''{R}` syntax

```{r include=TRUE}
sns<- import('seaborn')
plt<- import('matplotlib.pyplot')
pd <- import('pandas')
```
## Importing the dataset

Before manipulating anything we need a dataset, of course,In this case we will use the system’s `AirPassengers` dataset. Note that we are using R syntax to create this initial dataset and later we'll go over on how to interact with in in *Python*.

```{r}
df <- datasets::AirPassengers
df1<-data.frame(tapply(df, list(year= floor(time(df)), month = month.abb[cycle(df)]),c))
df1<- df1[month.abb]
df1
```



## Creating the plots

Following, you can pull in from R your data frame into python using the syntax below. Note that for every R object (variable, list, tupple, data frame) you need to let python know that is available in R so it finds it. Otherwise you will run an error that the object has not been assigned.


```{python}
import pandas as pd
df1 = pd.DataFrame(r.df1)
print(df1)
```

```{python}
import matplotlib.pyplot as plt
```

Lets build the plot using python syntax
```{python}
import seaborn as sns
sns.heatmap(df1, fmt='g', cmap='viridis')
plt.show()
```

**Success!**

Lets create pairplots

```{python}
sns.pairplot(r.iris, hue = 'Species')
plt.show()
```
## Alternative method to run python within R


```{r evaluate=FALSE, include=TRUE}
sns$heatmap(r_to_py(df1), fmt='g', cmap='viridis')
plt$show()
```
Note that using r_to_py has the same effect than using r.df1. Is the way we have to tell python that we will be using an R object.
```{r evaluate=FALSE, include=TRUE}
sns$pairplot(r_to_py(iris), hue='Species')
plt$show()
```

That’s it! You have now two options to interact with R and python. I still prefer [googlecollab](colab.research.google.com) but this works fine as well.

### References

1. [Full explanation](https://github.com/eduardorod1229/Notes-and-tutorials/blob/master/How-to-use-python-in-R_files/How-to-use-python-in-R.md)
1. [Python Seaborn Plots in R using reticulate](https://towardsdatascience.com/python-seaborn-plots-in-r-using-reticulate-fb59cebf61a7)
1. [Reticulate documentation](https://www.rdocumentation.org/packages/reticulate/versions/1.16)
1. [Calling python from R](https://cran.r-project.org/web/packages/reticulate/vignettes/calling_python.html)
1. [Anaconda Documentation](https://docs.anaconda.com/)
