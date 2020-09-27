
## How to use python in R

#### J. Eduardo Rodriguez Almaraz


To see the full images and dataset see the [html file](https://github.com/eduardorod1229/Notes-and-tutorials/blob/master/How-to-use-python-in-R.md)

## Importing libraries using R syntax

* For this to work you need to download and install the `reticulate` library and either [miniconda](https://docs.conda.io/en/latest/miniconda.html) or [anaconda](https://www.anaconda.com) software
With the following code you can import the python libraries as "R objects" which will appear in the **environment** section of R studio so for an RMarkdow you need to include the `'''{R}` syntax

```{r include=TRUE}
sns<- import('seaborn')
plt<- import('matplotlib.pyplot')
pd <- import('pandas')
```
If the libraries are correctly imported then they will shouw up in the *Global environment* pane as "Modules"

## Importing the dataset

Before manipulating anything we need a dataset, of course,In this case we will use the system’s `AirPassengers` dataset. Note that we are using R syntax to create this initial dataset and later we'll go over on how to interact with in in *Python*.

```{r}
df <- datasets::AirPassengers
df1<-data.frame(tapply(df, list(year= floor(time(df)), month = month.abb[cycle(df)]),c))
df1<- df1[month.abb]
df1
```

Now we have the data set you can explore it in your *global environment* window or by using the `df1` command in the console or script.

## Creating the plots

Following, you can pull in from R your data frame into python using the syntax below. Note that for every R object (variable, list, tupple, data frame) you need to let python know that is available in R so it finds it. Otherwise you will run an error that the object has not been assigned.

I choose to do this method given that R markdown will create the figures and you will be able to visualize them but you wouldn’t be able to see them in the final document if python syntax is used WITHIN R Hence, the chunk below begins with {python} to specify that we will be using python syntax.
You can include R code in the document as follows:

```{python}
import pandas as pd
df1 = pd.DataFrame(r.df1)
print(df1)
```
The print command is just to show how we are really importing the same data into python.
We first need to import the libraries. I am unsure if each library needs its own line but it seems that R handles that better than a whole chunk with all the libraries….Unfortunate

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
Up to importing the dataset will be the same. To create the plots in python.

* Begin by telling Rmarkdown that you will be using R syntax `{r}`

```{r evaluate=FALSE, include=TRUE}
sns$heatmap(r_to_py(df1), fmt='g', cmap='viridis')
plt$show()
```
Note that using r_to_py has the same effect than using r.df1. Is the way we have to tell python that we will be using an R object.
```{r evaluate=FALSE, include=TRUE}
sns$pairplot(r_to_py(iris), hue='Species')
plt$show()
```

That’s it! You have now two options to interact with R and python. I still prefer googlecollab but this works fine as well.

An additional note is that you have to install the *python* libraries in *R* before using it you can do this by typing `py_install('name of library')` in the console. You only have to do this once. If this doesn't work be sure to specify your path in the set up of the RMarkdown document

Be sure to read the reticulate documentation. This tutorial was done following instructions from [towards data science](https://towardsdatascience.com/python-seaborn-plots-in-r-using-reticulate-fb59cebf61a7).
