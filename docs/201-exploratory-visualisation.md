# (PART\*) Data analysis {-}



# Exploratory visualisation

<br/><small>*This chapter is currently a draft.*</small>

<br/><small><a href="javascript:if(window.print)window.print()">Print this chapter</a></small>


This chapter showcases an exploratory analysis of the distribution of people aged 20 to 24 in Leicester, using the `u011` variable from the [2011 Output Area Classification (2011OAC)](https://github.com/geogale/2011OAC) dataset introduced in [chapter 4](https://sdesabbata.github.io/r-for-geographic-data-science/table-operations.html#read-and-write-data). 

Before continuing, create a new R project in RStudio, and upload the `2011_OAC_Raw_uVariables_Leicester.csv` file to the project folder. Create new RMarkdown document within that project to replicate the analysis in this document. Once the document is set up, start by adding an R code snipped including the code below, which is loads the 2011OAC dataset and the libraries used for this chapter.


```r
library(tidyverse)
library(knitr)
leicester_2011OAC <- read_csv("2011_OAC_Raw_uVariables_Leicester.csv")
```





## GGlot2 recap

As seen in the introductory chapter, the [`ggplot2` library](https://ggplot2.tidyverse.org) is part of the Tidyverse, and it offers a series of functions for creating graphics **declaratively**, based on the concepts outlined in the Grammar of Graphics. While the `dplyr` library offers functionalities that cover *data manipulation* and *variable transformations*, the `ggplot2` library offers functionalities that allow to specify elements, define guides, and apply scale and coordinate system transformations.

- **Marks** can be specified in `ggplot2` using the [`geom_` functions](https://ggplot2.tidyverse.org/reference/index.html#section-layer-geoms).
- The mapping of variables (table columns) to **visual variables** can be specified in `ggplot2` using the [`aes` element](https://ggplot2.tidyverse.org/reference/aes.html).
- Furthermore, the `ggplot2` library:
    - automatically adds all necessary **guides** using default table column names, and additional functions can be used to overwrite the defaults;
    - provides a wide range of [`scale_` functions](https://ggplot2.tidyverse.org/reference/index.html#section-scales) that can be used to control the **scales** of all visual variables;
    - provides a series of [`coord_` fucntions](https://ggplot2.tidyverse.org/reference/index.html#section-coordinate-systems) that allow transforming the **coordinate system**. 

Check out the [`ggplot2` reference](https://ggplot2.tidyverse.org/reference/index.html) for all the details about the functions and options discussed below.

## Data visualisation

### Distributions

We start the analysis with a simple histogram, to explore the distribution of the variable `u011`. RMarkdown allows specifying the height (as well as the width) of the figure as an option for the R snipped, as shown in the example typed out in plain text below.

````
```{r, echo=TRUE, message=FALSE, warning=FALSE, fig.height = 4}
leicester_2011OAC %>%
  ggplot2::ggplot(
    aes(
      x = u011
    )
  ) +
  ggplot2::geom_histogram(binwidth = 5) +
  ggplot2::theme_bw()
```
````

The snipped and barchart is included in output documents, as shown below.


```r
leicester_2011OAC %>%
  ggplot2::ggplot(
    aes(
      x = u011
    )
  ) +
  ggplot2::geom_histogram(binwidth = 5) +
  ggplot2::theme_bw()
```

<img src="201-exploratory-visualisation_files/figure-html/unnamed-chunk-3-1.png" width="672" />

If we aim to explore how that portion of the population is distributed among the different supergroups of the 2011OAC, there are a number of charts that would allow us to visualise that relationship. 

For instance, the barchart above can be enhanced through the use of the visual variable colour and the `fill` option. The code creates a stacked barchart where sections of each bar are filled with the colour associated with a 2011OAC supergroup. 


```r
leicester_2011OAC %>%
  ggplot2::ggplot(
    aes(
      x = u011,
      fill = fct_reorder(supgrpname, supgrpcode)
    )
  ) +
  ggplot2::geom_histogram(binwidth = 5) +
  ggplot2::ggtitle("Leicester's young adults") +
  ggplot2::labs(
    fill = "2011 Output Area\nClassification\n(supergroups)"
  ) +
  ggplot2::xlab("Residents aged 20 to 24") +
  ggplot2::ylab("Count") +
  ggplot2::scale_fill_manual(
    values = c("#e41a1c", "#f781bf", "#ff7f00", "#a65628", "#984ea3", "#377eb8", "#ffff33")
  ) +
  ggplot2::theme_bw()
```

<img src="201-exploratory-visualisation_files/figure-html/unnamed-chunk-4-1.png" width="672" />

However, the graphic above is not extremely clear. A boxplot and a violin plot created from the same data are shown below. In both cases, the parameter `axis.text.x` of the function theme is set to `element_text(angle = 90, hjust = 1)` in order to orientate the labels on the x-axis vertically, as the supergroup names are rather long, and they would overlap one-another if set horizontally on the x-axis. In both cases, the option `fig.height` of the R snippet in RMarkdown should be set to a higher value (e.g., `5`) to allow for sufficient room for the supergroup names.


```r
leicester_2011OAC %>%
  ggplot2::ggplot(
    aes(
      x = fct_reorder(supgrpname, supgrpcode),
      y = u011,
      fill = fct_reorder(supgrpname, supgrpcode)
    )
  ) +
  ggplot2::geom_boxplot() +
  ggtitle("Leicester's young adults") +
  ggplot2::labs(
    fill = "2011 Output Area\nClassification\n(supergroups)"
  ) +
  ggplot2::xlab("2011 Output Area Classification (supergroups)") +
  ggplot2::ylab("Residents aged 20 to 24") +
  ggplot2::scale_fill_manual(
    values = c("#e41a1c", "#f781bf", "#ff7f00", "#a65628", "#984ea3", "#377eb8", "#ffff33")
  ) +
  ggplot2::theme_bw() +
  ggplot2::theme(axis.text.x = element_text(angle = 90, hjust = 1))
```

<img src="201-exploratory-visualisation_files/figure-html/unnamed-chunk-5-1.png" width="672" />


```r
leicester_2011OAC %>%
  ggplot2::ggplot(
    aes(
      x = fct_reorder(supgrpname, supgrpcode),
      y = u011,
      fill = fct_reorder(supgrpname, supgrpcode)
    )
  ) +
  ggplot2::geom_violin() +
  ggtitle("Leicester's young adults") +
  ggplot2::labs(
    fill = "2011 Output Area\nClassification\n(supergroups)"
  ) +
  ggplot2::xlab("2011 Output Area Classification (supergroups)") +
  ggplot2::ylab("Residents aged 20 to 24") +
  ggplot2::scale_fill_manual(
    values = c("#e41a1c", "#f781bf", "#ff7f00", "#a65628", "#984ea3", "#377eb8", "#ffff33")
  ) +
  ggplot2::theme_bw() +
  ggplot2::theme(axis.text.x = element_text(angle = 90, hjust = 1))
```

<img src="201-exploratory-visualisation_files/figure-html/unnamed-chunk-6-1.png" width="672" />



### Relationships

The first barchart above seems to illustrate that the distribution might be skewed towards the left, with most values seemingly below 50. However, that tells only part of the story about how people aged 20 to 24 are distributed in Leicester. In fact, each Output Area (OA) has a different total population. So, a higher number of people aged 20 to 24 living in an OA might be simply due to the OA been more populous than others. Thus, the next step is to compare `u011` to `Total_Population`, for instance, through a scatterplot such as the one below.


```r
leicester_2011OAC %>%
  ggplot2::ggplot(
    aes(
      x = Total_Population,
      y = u011,
      colour = fct_reorder(supgrpname, supgrpcode)
    )
  ) +
  ggplot2::geom_point(size = 0.5) +
  ggplot2::ggtitle("Leicester's young adults") +
  ggplot2::labs(
    colour = "2011 Output Area\nClassification\n(supergroups)"
  ) +
  ggplot2::xlab("Total number of residents") +
  ggplot2::ylab("Residents aged 20 to 24") +
  ggplot2::scale_y_log10() +
  ggplot2::scale_colour_brewer(palette = "Set1") +
  ggplot2::scale_colour_manual(
    values = c("#e41a1c", "#f781bf", "#ff7f00", "#a65628", "#984ea3", "#377eb8", "#ffff33")
  ) +
  ggplot2::theme_bw()
```

<img src="201-exploratory-visualisation_files/figure-html/unnamed-chunk-7-1.png" width="672" />



## Exercise 304.1

**Question 304.1.1:** Which one of the boxplot or violin plot above do you think better illustrate the different distributions, and what do the two graphics say about the distribution of people aged 20 to 24 in Leicester? Write a short answer in your RMarkdown document (max 200 words).

**Question 304.1.2:** Create a jittered points plot (see [`geom_jitter`](https://ggplot2.tidyverse.org/reference/geom_jitter.html)) visualisation illustrating the same data shown in the boxplot and violin plot above.

**Question 304.1.3:** Create the code necessary to calculate a new column named `perc_age_20_to_24`, which is the percentage of people aged 20 to 24 (i.e., `u011`) over total population per OA `Total_Population`, and create a boxplot visualising the distribution of the variable per 2011OAC supergroup.


---

<small>by [Stefano De Sabbata](https://sdesabbata.github.io/) -- text licensed under the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/), contains public sector information licensed under the [Open Government Licence v3.0](http://www.nationalarchives.gov.uk/doc/open-government-licence), code licensed under the [GNU GPL v3.0](https://www.gnu.org/licenses/gpl-3.0.html).</small>