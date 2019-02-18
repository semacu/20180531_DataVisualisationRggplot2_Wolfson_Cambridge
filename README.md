
## Data visualisation with R and ggplot2 

<img align="right" src=../../../20180315_IntroductionToR_Wolfson_Cambridge/blob/master/images/R_logo.png width="150">

- Date: 31st May 2018, 6 - 7pm
- Series: Wolfson College [Skills for Academic Success](https://www.wolfson.cam.ac.uk/study-skills)
- Location: [Gatsby room](http://www.wolfson.cam.ac.uk/tour/chancellorscentre), [Wolfson college](https://goo.gl/maps/aR6a5FWrLoR2), University of Cambridge, UK
- Trainer: Sergio Martínez Cuesta
- Register [here](https://www.eventbrite.co.uk/e/skills-for-academic-success-data-visualisation-using-r-and-ggplot2-tickets-45290341631)



### Overview

This course provides a short beginners introduction to data visualisation using the R programming language and software environment for statistical computing and graphics. Sergio will demonstrate basic examples on how to import data, perform different types of plots and export graphics using R standard functions and the library ggplot2. Everybody is welcome; if you would like to follow along with your laptop, please bring R and RStudio [downloaded and installed](https://www.rstudio.com/products/rstudio/download/) before the session.



### Outline

- Motivation
- Getting started
- Import data into R
- Basic plotting
- Exercise 1
- Advanced plotting using the ggplot2 library
- Export graphics

For a basic introduction to R functionality, check out our [basic R course](../20180315_IntroductionToR_Wolfson_Cambridge/).

These short courses are inspired by the [R crash course](https://github.com/bioinformatics-core-shared-training/r-crash-course) developed by [Mark Dunning](https://github.com/markdunning), [Laurent Gatto](https://github.com/lgatto) and others.



### Motivation

- R is one of the most widely-used programming languages for data analysis, statistics and visualisation in academia and industry.
- It is open-source and available in all platforms (Mac, Linux and Windows)
- Supported by a broad community of software developers and researchers who contribute R packages and libraries to many fields of research 
- It facilitates reproducibility in research and integration of all your analyses in individual scripts
- Easy to write documentation and code together using a free environment like [RStudio](https://www.rstudio.com/)



### Getting started

- Open RStudio, e.g. go to `Finder` -> `Applications` and click on `Rstudio`

To download today's workshop:

- Go to your web browser e.g. Firefox and type: https://tinyurl.com/2018-DataVisR-Wolfson
- Click on `DataVisR.zip`, then press `Download` and save the file in your preferred folder, e.g. your Desktop
- Go to the folder where you saved `DataVisR.zip` and uncompress it, e.g. in Mac just double-click on `DataVisR.zip`. Only then, the folder `DataVisR` will appear.
- The folder `DataVisR` contains two files:
    - `DataVisR.Rmd` - the code for today's session
    - `patient-data-cleaned.csv` - the dataset that we will be exploring

Now, go back to RStudio:

- Click on `File` -> `Open File` and select `DataVisR.Rmd`
- You are all set to go now :)

Also, in case you are not familiar with RStudio, a quick recap:

- RStudio interface is composed of four panels, in anti-clockwise sense:
  - Top-left: scripts panel 
  - Bottom-left: R console
  - Bottom-right: plots, packages and help
  - Top-right: log panel

- You are now looking at the scripts panel. We will be using the R console below to interact with R during the workshop
- Blocks of code in RStudio are often written using the format **R markdown**, which allows mixed plain text and R code together within the same [document](https://rmarkdown.rstudio.com/)
- Each line of R code inside a block can be executed by clicking on the line and pressing CMD + ENTER (Mac) or CTRL + ENTER (Windows and Linux), e.g.:

```{r eval=FALSE}
print("R is fun!")
```

Alternatively, to execute the entire block, click on the green arrow tip on the right-hand side of the block.

```{r eval=FALSE}
3 + 1
```

- You can add a new block of code by selecting `R` in the `Insert` menu or by typing the following syntax directly:

```{r}
# R code goes in here
```



### Import data into R

We will use a small made-up dataset which is often used for [training purposes](https://www.rpubs.com/Shoaib29/323077). It contains information about 100 lung cancer patients aged 42-44 from different states in the US. We have saved these data as a comma-separated values (CSV) file `patient-data-cleaned.csv`, which can easily be opened using software like Excel. In R, use the `read.csv()` function to import the data:

```{r eval=FALSE}
patient_data <- read.csv("/Users/martin03/Desktop/DataVisR/patient-data-cleaned.csv") # copy here the path to the file
```

If you have trouble finding the exact path to `patient-data-cleaned.csv`, use the function `file.choose()` to open a dialogue box and browse through the directories to reach the file:

```{r eval=FALSE}
file.choose()
```

The path will then be displayed in R and you can copy it into the `read.csv()` command above.

The object `patient_data` is known as a data frame in R. To explore its contents:

```{r eval=FALSE}
# Dimensions (rows and columns)
dim(patient_data)

# Viewing contents
View(patient_data)

# Structure of the data frame
str(patient_data)

# Summary of all data frame contents
summary(patient_data)
```



### Basic plotting

Simple plotting functions are available in the base R distribution (histograms, barplots, boxplots, scatterplots ...). All that is required as input are vectors of data, e.g. columns in your data frame.

**Histograms** are often used to have an overview of the distribution of continuous data:

```{r eval=FALSE}
hist(patient_data$BMI)
hist(patient_data$Weight)
```

**Barplots** are useful when you have counts of categorical data:

```{r eval=FALSE}
barplot(table(patient_data$Race))
barplot(table(patient_data$Sex))
barplot(table(patient_data$Smokes))
barplot(table(patient_data$State), las=2, cex.names=0.7) # 'las=2' changes the x-axis labels to horizonal and 'cex.names=0.7' changes the size
barplot(table(patient_data$Grade))
barplot(table(patient_data$Overweight))
```

**Boxplots** are good when comparing distributions Here the `~` symbol sets up a formula, the effect of which is to put the categorical variable on the x-axis and continuous variable on the y-axis -> `boxplot(y ~ x)`

```{r eval=FALSE}
boxplot(patient_data$BMI ~ patient_data$Grade)
boxplot(patient_data$BMI ~ patient_data$Overweight)

boxplot(patient_data$Weight ~ patient_data$Overweight)
```

**Scatter plots** are useful when representing two continuous variables. Here -> `plot(x, y)`:

```{r eval=FALSE}
plot(patient_data$Weight, patient_data$BMI)
```

To enhance the appearance of your plots, many different ways of customisation are possible:

- **Colours**: `col` argument. To get a full list of possible colours type `colours()`, or check this [online reference](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf).
- **Point type**: `pch`
- **Axis labels**: `xlab` and `ylab`
- **Plot title**: `main`
- ... and many others: see `?plot` and `?par` for more options

```{r eval=FALSE}
# linear regression
plot(patient_data$Weight, patient_data$BMI, col="red", pch=16, xlab="Weight (kg)", ylab="BMI", main="US patient data")
abline(lm(patient_data$BMI ~ patient_data$Weight), col="blue")

# polynomial regression
quadratic.model <-lm(patient_data$BMI ~ patient_data$Weight + I(patient_data$Weight^2))
plot(patient_data$Weight, patient_data$BMI, col="red", pch=16, xlab="Weight (kg)", ylab="BMI", main="US patient data")
lines(sort(patient_data$Weight), fitted(quadratic.model)[order(patient_data$Weight)], col = "darkgreen")
```

The arguments can also be used for other plotting functions!

```{r eval=FALSE}
boxplot(patient_data$BMI ~ patient_data$Overweight, col=c("red", "green"), xlab="Overweight patient?", ylab="BMI", main="US patient data")
```

To explore other types of plots using R standard functions, have a look [here](https://www.statmethods.net/graphs/index.html). There are dedicated R libraries e.g. [ggplot2](http://ggplot2.tidyverse.org/index.html) to do more sophisticated plotting.



### Exercise 1

1. Any differences of BMI between Smokers and Non-Smokers? (hint: try `boxplot`)
2. Visualise the relationship between the Height and Weight of the patients
3. A small trick: if you attach the data.frame `patient_data` as follows, then you will only need the column name without the '$' notation:

```{r eval=FALSE}
attach(patient_data)
plot(Weight, BMI)
```



### Advanced plotting using the ggplot2 library

The [ggplot2 library](http://ggplot2.tidyverse.org/) offers a powerful graphics language for creating elegant and complex plots. It is particularly useful when creating publication-quality graphics.

The key to understanding ggplot2 is thinking about a figure in layers (e.g. data points, axes and labels, legend). This idea may be familiar to you if you have used image editing programs like Photoshop, Illustrator or Inkscape, where you can ungroup the figure into its different components.


#### Load ggplot2

There are two ways to do this:

- Click on the `Packages` tab in the bottom-right RStudio panel and search for `ggplot2`, then tick its box. If you can't find it, then click on `Install` and type `ggplot2` inside the Packages box. Leaving the rest on default, click on `Install`. Once installed, then tick the box.

- Run `library(ggplot2)` in the console. If you get a message like `Error in library("ggplot2") : there is no package called ‘ggplot2’` then run `install.packages("ggplot2")` in the console. Once the installation is finished, run `library(ggplot2)` again.


#### Example

Let's begin with the scatterplot of Weight and Height. 

First, loading ggplot2 library:

```{r eval=FALSE}
library("ggplot2")
```

The first "global" layer requires the definition of the dataset, and the x and y axes:

```{r eval=FALSE}
ggplot(data = patient_data, aes(x = Weight, y = Height))
```

In the second layer, we need to tell ggplot how we want to visually represent the data (scatterplot, boxplot, barplot ...). For a scatterplot, we need **geom_point()**:

```{r eval=FALSE}
ggplot(data = patient_data, aes(x = Weight, y = Height)) +
geom_point()
```

Another aes (aesthetic) property we can modify is the point *color*, e.g. to change the color depending on the grade of the disease:

```{r eval=FALSE}
ggplot(data = patient_data, aes(x = Weight, y = Height, col = as.factor(Grade))) +
geom_point()
```



### Export graphics

When running commands directly in the interactive console (bottom-left panel), plots can be exported using the **Plots** tab in RStudio (bottom-right panel). Click on `Export` -> `Save as PDF ...`.

When plotting using R standard graphics, you can also save plots to a file calling the `pdf()` or `png()` functions before executing the code to create the plot:
 
```{r eval=FALSE}
pdf("/Users/martin03/Desktop/DataVisR/BMIvsWeight.pdf")
plot(patient_data$Weight, patient_data$BMI, col="red", pch=16, xlab="Weight (kg)", ylab="BMI", main="US patient data")
abline(lm(patient_data$BMI ~ patient_data$Weight), col="blue")
dev.off()
```

The `dev.off()` line is important; without it you will not be able to view the plot you have created.

If you use ggplot2, the syntax is more concise:

```{r eval=FALSE}
gg<- ggplot(data = patient_data, aes(x = Weight, y = Height, col = as.factor(Grade))) +
geom_point()
ggsave("/Users/martin03/Desktop/DataVisR/HeightvsWeight.pdf")
```


That's it! Enjoy R!



### Questions?

Feedback / questions about the course, please email Sergio (sermarcue@gmail.com).



### References and resources

Blogs:
- [Getting started with data visualization in R using ggplot2](http://www.storybench.org/getting-started-data-visualization-r-using-ggplot2/)
- [End-to-end visualization using ggplot2](https://rviews.rstudio.com/2017/08/14/end-to-end-visualization-using-ggplot2/)
- [ggplot2 - Easy way to mix multiple graphs on the same page](http://www.sthda.com/english/wiki/ggplot2-easy-way-to-mix-multiple-graphs-on-the-same-page)
- [Rookie mistakes and how to fix them when making plots of data](http://stuartlee.org/2018/04/14/content/post/2018-04-14-rookie-mistakes/)
- [BBC Visual and Data Journalism cookbook for R graphics](https://bbc.github.io/rcookbook/)

Books:

- [Cookbook for R](http://www.cookbook-r.com/)
- [R for Data Science](http://r4ds.had.co.nz/)
- [Data Visualization for Social Science. A practical introduction with R and ggplot2](http://socviz.co/)
- [ggplot2: Elegant Graphics for Data Analysis (Use R!)](https://www.amazon.com/ggplot2-Elegant-Graphics-Data-Analysis/dp/0387981403)
- [R packages](http://r-pkgs.had.co.nz/)
- [plotly for R](http://plotly-book.cpsievert.me/)
- [bookdown: Authoring Books and Technical Documents with R Markdown](https://bookdown.org/yihui/bookdown/)

Courses:

- [CRUK-CI R crash course](https://bioinformatics-core-shared-training.github.io/r-crash-course/)
- [R for Reproducible Scientific Analysis](http://swcarpentry.github.io/r-novice-gapminder/)
- [Karl Broman's mini tutorials](http://kbroman.org/pages/tutorials.html)
- [Basic statistics and data handling with R](https://github.com/cambiotraining/stats-intro)
- [Scripting for data analysis (with R)](https://github.com/mrtnj/scripting_for_data_analysis)
- [An Introduction to Solving Biological Problems with R](http://cambiotraining.github.io/r-intro/)
- [Data Analysis and Visualisation using R](http://bioinformatics-core-shared-training.github.io/r-intermediate/): including dplyr and ggplot2
- Babraham institute basic/advanced R and ggplot2 [courses](http://www.bioinformatics.babraham.ac.uk/training/)
- R object-oriented programming and package development, [link1](http://lgatto.github.io/TeachingMaterial/) and [link2](http://logic.sysbiol.cam.ac.uk/teaching/advancedR/)
- [R course content for the CODATA-RDA Research Data Science Summer School](https://github.com/marioa/trieste)
- [Data carpentry course for biologists](https://jabberwocky.weecology.org/2016/11/14/fork-our-course-a-semester-long-data-carpentry-course-for-biologists/) by Ethan White
- [Cambridge's Data carpentry using R](https://tavareshugo.github.io/2017-09-11-cambridge/)

Perspectives:

- [Bioconductor: A test drive of a DNA-analysis toolkit in the cloud](https://www.nature.com/articles/d41586-017-07833-1)
- [Scientific computing: code alert](https://www.nature.com/nature/journal/v541/n7638/full/nj7638-563a.html)

Tutorials:

- [The ggplot flipbook](https://evamaerey.github.io/ggplot_flipbook/ggplot_flipbook_xaringan.html#1)


### Acknowledgements

Sergio is a University of Cambridge Data Champion funded by a Jisc research data [fellowship](https://www.jisc.ac.uk/) to develop research data training activities for researchers. He does research in bioinformatics and computational biology within the [Balasubramanian laboratories](http://www.balasubramanian.co.uk/) funded by the Wellcome Trust at the University of Cambridge.



### License

This work is distributed under a Creative Commons [CC0 license](https://en.wikipedia.org/wiki/Creative_Commons_license#CC0). No rights reserved.

Our sponsors:

<p align="center">
<img src=../../../20171024_GitHub_Chemistry_Cambridge/blob/master/images/UniversityCambridge_logo.png height="50"> <img src=../../../20171024_GitHub_Chemistry_Cambridge/blob/master/images/CRUKCI_logo.jpg height="50"> <img src=../../../20171024_GitHub_Chemistry_Cambridge/blob/master/images/Jisc_logo.png height="50"> <img src=../../../20171024_GitHub_Chemistry_Cambridge/blob/master/images/WellcomeTrust_logo.jpg height="50">
</p>


