---
title: "iNaturalist Talk"
author: "Michele Wiseman"
date: "2022-10-02"
---

# Using R for powerful iNaturalist data wrangling
Hello there, my name is Michele Wiseman and I'm a PhD candidate in Botany and Plant Pathology at Oregon State University. Aside from being a PhD student, I'm also a serious mycophile (lover of fungi).  When I first discovered my passion for fungi (ca. 2005), I dove into whatever resources I could find to learn more about them and how I could find them including reading online forums (e.g. [Shroomery](www.shroomery.com)), mycological books (like [Mushrooms Demystified](https://www.goodreads.com/en/book/show/415949.Mushrooms_Demystified), studying observations on [MushroomObserver](www.mushroomobserver.org), and even looking at specimens in my local mycological herbarium, the [Oregon State Collection](https://bpp.oregonstate.edu/herbarium/). *I quickly learned that the most efficient way to find fungi was by mapping out where observations occur using lat/long coordinates.* Alas, I started noting the township information and coordinates on MushroomObserver observations and herbarium packets and would use (**gasp**) USGS maps to discover phylogeographic trends. 

Fast forward to today: after 17 years of hunting and studying fungi, I know a smidge more about fungal ecology and biology; however, I still routinely map out observational data for both professional endeavors (e.g. biodiversity studies) and for personal endeavors (to up my mushroom hunting game). I can now accomplish what used to take weeks of data collection and curation in a matter of minutes using modern data wrangling and visualization tools. 

Today, I'd like to teach you about the power of these tools so you too can up your mushroom hunting game (*and will hopefully realize the importance of citizen-scientist observations*).

## Table of Contents

* [Introduction to iNaturalist](#introduction-to-inaturalist)
* [Introduction to R](#introduction-to-r)
  * [User-friendly R coding with Rstudio](#User-friendly-R-coding-with-Rstudio)
  * [Comprehensive-R-Archive-Network-(CRAN)](#Comprehensive-R-Archive-Network-(CRAN))
  * [Loading and Installing Packages](#loading-and-installing-packages)
* [Code and Instructions for Mapping iNaturalist Data in R](#mapping-inat-in-r)
* [Code for Counting and Visualizing Papers Discussing iNaturalist](#inat-keyword)
* [Resources for New Coders](#beginning-coding)
* [R iNaturalist Resources](#resources)
* [Books](#books)
* [Other](#other)
* [Contact](#contact)
* [Acknowledgements](#acknowledgements)

## Introduction to iNaturalist
[Back to top](#table-of-contents)
[iNaturalist](https://www.inaturalist.org/)  is a social network of naturalists, citizen scientists, and biologists built on the concept of mapping and sharing observations of biodiversity across the globe. In 2017, iNaturalist usage exploded due to the release of a computer vision tool that automatically suggests species IDs from uploaded pictures. As computer-vision aided identification lowered the barrier to entry, more and more citizen scientists started using the tool. Consequently, the use of iNaturalist data in conservation, biodiversity, hybridization, phylogeography, etc., research also exploded. 

![iNaturalist citation growth](images/iNatPaperGrowth.png)
 

[Getting started in iNaturalist](https://www.inaturalist.org/pages/getting+started).

## Introduction to R 
[Back to top](#table-of-contents)

Note: This first section's text is quoted directly from **An Introduction to R** written by Bill Venables and David M. Smith. [Download the pdf](https://cran.r-project.org/doc/manuals/r-release/R-intro.pdf) for a more comprehensive introduction to R.

[R](https://www.r-project.org/) is an integrated suite of software facilities for data manipulation, calculation and graphical display. Among other things it has:

* an effective data handling and storage facility,
* a suite of operators for calculations on arrays, in particular matrices,
* a large, coherent, integrated collection of intermediate tools for data analysis,
* graphical facilities for data analysis and display either directly at the computer or on hardcopy, and
* a well developed, simple and effective programming language (called ‘S’) which includes conditionals, loops, user defined recursive functions and input and output facilities. (Indeed most of the system supplied functions are themselves written in the S language.)

The term “environment” is intended to characterize it as a fully planned and coherent system, rather than an incremental accretion of very specific and inflexible tools, as is frequently the case with other data analysis software. R is very much a vehicle for newly developing methods of interactive data analysis. It has developed rapidly, and has been extended by a large collection of packages. However, most
programs written in R are essentially ephemeral, written for a single piece of data analysis.

### User-friendly R coding with Rstudio
[Back to top](#table-of-contents)

[RStudio](https://www.rstudio.com/products/rstudio/) is an integrated development environment (IDE) for R. It includes a console, syntax-highlighting editor that supports direct code execution, as well as tools for plotting, history, debugging and workspace management. **For most of my R coding, I use Rstudio and I recommend you do the same due to its ease-of-use**. 

There's a nice list of Rstudio tutorials [here](https://education.rstudio.com/learn/beginner/).

### Comprehensive R Archive Network (CRAN)
[Back to top](#table-of-contents)

R is an open source tool which offers great flexibility in customization of code, but can also present issues with package compatibility (e.g. conflicting code in between libraries) and poor package management (i.e. things get outdated and abandoned). You can always check the status of a package by looking at it's *Comprehensive R Archive Network (CRAN)* repository information. The CRAN page for a package will tell you when it was last updated, any necessary dependencies for the package, will link you to the reference manual, and will link you to a page where you can submit bug fix requests. *Most packages are on [CRAN](https://cran.r-project.org/)* or are directly hosted on [GitHub](https://github.com/).

The CRAN pages are linked below for today's required packages:

* [tidyverse](https://cran.r-project.org/web/packages/tidyverse/index.html)
* [rinat](https://cran.r-project.org/web/packages/rinat/index.html)
* [lubridate](https://cran.r-project.org/web/packages/lubridate/index.html)
* [maps](https://cran.r-project.org/web/packages/maps/index.html)
* [ggmaps](https://cran.r-project.org/web/packages/ggmaps/index.html)
* [janitor](https://cran.r-project.org/web/packages/janitor/index.html)

### Loading required packages
[Back to top](#table-of-contents)

To best utilize the power of R, you must install and load the accessory packages. To install packages, run the command `install.packages("[libraryname]")`. For example, to install `tidyverse` I would run `install.packages("tidyverse")`. If you need to install many packages at once, you can make a one line command that creates a [vector](https://www.tutorialspoint.com/r/r_vectors.htm) of the packages you want and run the installs them. For example, to install the packages we need: `install.packages(c("tidyverse", "rinat", "lubridate","maps", "janitor", "ggmap"))`

As the packages install, you may be prompted to also install dependencies. Say yes to any dependencies.

Once your packages have installed, load them using the command `library([packagename])`. As above, you can combine them all in one line of code if you wish, though I prefer to keep them separated for ease of customization.

## Using R for powerful iNaturalist data analyses
[Back to top](#table-of-contents)
 
First, load and install required packages.

```r, setup
# if you need to install any packages, remove the hashtag (i.e. uncomment) and then run. 
# install.packages(c("tidyverse", "rinat", "lubridate","maps", "janitor", "ggmap"))

# the required packages
library(tidyverse)         # cleaning data
library(rinat)             # importing iNaturalist data
library(lubridate)         # changing data format easily
library(maps)              # database of maps
library(janitor)           # helpful data tidying tools
library(ggmap)             # powerful mapping tool

### optional - for aesthetics ###
#library(ggthemes)      # expanded themes, optional
```

---

## iNaturalist publications overtime
[Back to top](#table-of-contents)

Scrape Google Scholar to count iNaturalist publications between inception (2008) and today. Python script [here](https://github.com/Pold87/academic-keyword-occurrence). 

To run: `python extract_occurrences.py 'iNaturalist' 2008 2022`

The output will print to your screen and you can either save it directly to a new file by copy+paste or using a redirect (>).

Make a dataframe with the output either manually (as I have) or by importing your table (load the `readr` library and then run `read.csv([yourfilepath.csv`])).

```r, iNaturalist publications overtime
library(ggdark)             # for the dark theme

year <- c("2008", "2009", "2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017", "2018", "2019", "2020", "2021")
papers <- c("33", "47", "35", "53", "74", "67", "148", "201", "309", "402", "645", "1120", "1730", "2800")
df <- data.frame(year, papers)

ggplot(df) +
  geom_col(aes(x=year, y=as.numeric(papers)), fill = "#80AA33") +
  ggtitle("New Scientific Peer-Reviewed Papers Referencing iNaturalist") +
  scale_fill_gradientn(colours = rainbow(5))+
  ylab("Number of Papers") +
  dark_theme_classic() +
    theme(
    axis.title.x = element_blank(),
    plot.title = element_text(face = "bold", size = 14),
    axis.text = element_text(face = "bold", color = "white", size = 10)
  )

ggsave("iNaturalistpapers.tiff",
       dpi = 300, 
       width = 7,
       height = 5,
       units = "in")

```

I added the text and arrow in powerpoint, but you can easily add it in ggplot using `ggtext`

![iNaturalist citation growth](images/iNatPaperGrowth.png)


---
Beginning coding resources
[Back to top](#table-of-contents)


---
## Contact?  
[Back to top](#table-of-contents)

Feel free to [email me](mailto:michele.wiseman@oregonstate.edu?subject=Mapping%20iNaturalist%20Observations) with questions, comments, or if you're interested in collaboration. Follow what I'm up to on my [website](https://mswiseman.github.io/), [twitter](https://twitter.com/mswiseman), and/or [iNat](https://www.inaturalist.org/people/mswiseman) page. 

---

## Acknowledgments
[Back to top](#table-of-contents)
I'd like to thank my PhD advisor (and often, general life mentor) **David Gent** who is endlessly supportive of eccentric projects and is incredibly patient, kind, and brilliantly insightful. I feel so lucky to be advised by you. I'd also like to thank my **Jessie Uehling**, **Dan Luoma**, and **Joey Spatafora** for keeping me in the fungal loop inspite of having strayed to the plant side for my dissertation work. Jessie has been especially encouraging of all my mapping pursuits. Thank you to **Ishika Kumbhakar** who is absolutely brilliant with GIS data and mapping and often comes in clutch when I'm troubleshooting difficult analyses. Finally, thank you to my close network of friends and family members who are so patient with my neverending endeavors: my mom (Judi Sanders), twin sister (Nicole Wiseman), brother (Michael Wiseman), and the super amazing mildew mafia (Teddy Borland, Briana Richardson, Steve Massie, Nanci Adair, Carly Cooperider, Nadine Wade, Liz Lopez, Tatum Clark).
