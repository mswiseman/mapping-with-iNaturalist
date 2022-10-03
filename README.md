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
 * [Mapping along a trail](#Mapping-iNaturalist-observations-along-a-trail)
* [Code for Counting and Visualizing Papers Discussing iNaturalist](#inat-keyword)
* [Resources](#resources)
* [Other](#other)
* [Contact](#contact)
* [Acknowledgements](#acknowledgements)

---

## Introduction to iNaturalist
[Back to top](#table-of-contents)

[iNaturalist](https://www.inaturalist.org/)  is a social network of naturalists, citizen scientists, and biologists built on the concept of mapping and sharing observations of biodiversity across the globe. In 2017, iNaturalist usage exploded due to the release of a computer vision tool that automatically suggests species IDs from uploaded pictures. As computer-vision aided identification lowered the barrier to entry, more and more citizen scientists started using the tool. Consequently, the use of iNaturalist data in conservation, biodiversity, hybridization, phylogeography, etc., research also exploded. 

![iNaturalist citation growth](images/iNatPaperGrowth.png)

Using iNaturalist is easy - simply capture the nature world, record any pertinent notes (e.g. location if your camera/phone doesn't capture lat/long coordinates), and upload to [iNaturalist](https://www.inaturalist.org/).  

** Using iNaturalist on your Phone **
First, download the iNaturalist application to your iPhone or Android phone. Then, follow the instructions below (provided by [iNaturalist](https://www.inaturalist.org/pages/getting+started)).

![iNaturalist using a phone](images/phone-observations.png)

** Using iNaturalist on the web**
Simply navigate to [iNaturalist](https://www.inaturalist.org/), sign up for an account, and then you're ready to make observations. 

![iNaturalist using the web](images/web-observations.png)

**For a more detailed tutorial, check out [getting started in iNaturalist](https://www.inaturalist.org/pages/getting+started).**

---

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

---

### User-friendly R coding with Rstudio
[Back to top](#table-of-contents)

[RStudio](https://www.rstudio.com/products/rstudio/) is an integrated development environment (IDE) for R. It includes a console, syntax-highlighting editor that supports direct code execution, as well as tools for plotting, history, debugging and workspace management. **For most of my R coding, I use Rstudio and I recommend you do the same due to its ease-of-use**. 

There's a nice list of Rstudio tutorials [here](https://education.rstudio.com/learn/beginner/).

---

### Comprehensive R Archive Network (CRAN)
[Back to top](#table-of-contents)

R is an open source tool which offers great flexibility in customization of code, but can also present issues with package compatibility (e.g. conflicting code in between libraries) and poor package management (i.e. things get outdated and abandoned). You can always check the status of a package by looking at it's Comprehensive R Archive Network (CRAN) repository information. The CRAN page for a package will tell you when it was last updated, any necessary dependencies for the package, will link you to the reference manual, and will link you to a page where you can submit bug fix requests. *Most packages are on [CRAN](https://cran.r-project.org/) or are directly hosted on [GitHub](https://github.com/).*

The CRAN pages are linked below for today's required packages:

* [ggmaps](https://cran.r-project.org/web/packages/ggmaps/index.html)
* [janitor](https://cran.r-project.org/web/packages/janitor/index.html)
* [lubridate](https://cran.r-project.org/web/packages/lubridate/index.html)
* [maps](https://cran.r-project.org/web/packages/maps/index.html)
* [rinat](https://cran.r-project.org/web/packages/rinat/index.html)
* [tidyverse](https://cran.r-project.org/web/packages/tidyverse/index.html)

There are also developmental versions which often contain coding examples on GitHub:

* [ggmap](https://github.com/dkahle/ggmap)
* [janitor](https://github.com/sfirke/janitor)
* [lubridate](https://github.com/tidyverse/lubridate)
* [maps](https://github.com/nextcloud/maps)
* [rinat](https://github.com/ropensci/rinat)
* [tidyverse](https://github.com/tidyverse/tidyverse)

---

### Loading required packages
[Back to top](#table-of-contents)

To best utilize the power of R, you must install and load the accessory packages. To install packages, run the command `install.packages("[libraryname]")`. For example, to install `tidyverse` I would run `install.packages("tidyverse")`. If you need to install many packages at once, you can make a one line command that creates a [vector](https://www.tutorialspoint.com/r/r_vectors.htm) of the packages you want and run the installs them. For example, to install the packages we need: `install.packages(c("tidyverse", "rinat", "lubridate","maps", "janitor", "ggmap"))`

As the packages install, you may be prompted to also install dependencies. Say yes to any dependencies.

Once your packages have installed, load them using the command `library([packagename])`. As above, you can combine them all in one line of code if you wish, though I prefer to keep them separated for ease of customization.

---

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

## Mapping iNaturalist observations along a trail
[Back to top](#table-of-contents)

Leading a hike and need a species list? You can manually use a bounding box to limit species to your trail using the iNaturalist web platform, but it's clunky and not precise. A really cool alternative is to make use of the [iNat trails](https://github.com/joergmlpts/iNat-trails) python script which maps out all observations along a trail. 

To start, download the [iNat trails](https://github.com/joergmlpts/iNat-trails) repository from github (instructions on downloading from github [here](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)). If you have git installed, this can be simply `git clone https://github.com/joergmlpts/iNat-trails`. 

In order to run `iNat-trails`, you must also install a few dependencies. Assuming you have Python 3.7 or later, you can do this in command line by typing:
```sh
sudo apt install --yes python3-pip python3-aiohttp python3-shapely
pip3 install folium
```
Once you've got all the dependencies installed, give one of the examples a test run by typing:
```sh
./inat_trails.py examples/Rancho_Canada_del_Oro.gpx
```
You're output should look like this: 

```
Reading 'examples/Rancho_Canada_del_Oro.gpx'...
Loaded 13 named roads and trails: Bald Peaks Trail, Canada Del Oro Cut-Off Trail, Canada Del Oro Trail, Casa Loma Road,
    Catamount Trail, Chisnantuk Peak Trail, Little Llagas Creek Trail, Llagas Creek Loop Trail, Longwall Canyon Trail,
    Mayfair Ranch Trail, Needlegrass Trail, Serpentine Loop Trail.
Loaded 2,708 iNaturalist observations of quality-grade 'research' within bounding box.
Excluded 1,694 observations not along route and 13 with low accuracy.
Loaded 829 taxa.
Waypoints written to './Rancho_Canada_del_Oro_Open_Space_Preserve_all_research_waypoints.gpx'.
Table written to './Rancho_Canada_del_Oro_Open_Space_Preserve_all_research_observations.html'.
Map written to './Rancho_Canada_del_Oro_Open_Space_Preserve_all_research_mapped_observations.html'.
```

Assuming things work, lets try a different trail. Pick one that you regularly recreate on. For this example, I'll look at a loop trail at the Lewisburg Saddle here in Corvallis, OR.  Navigate to [Caltopo](https://caltopo.com/) and search your trail (for me: `Lewisburg Saddle`). Next, use the line tool to trace your trail (note: this shouldn't be hard since the path autofits to the trail as you go). Save the trail with a name and export it as a GPX file. 

**Tracing and saving our trail**
| ![tracinglewisburgsaddle](images/line-tracing-saddle.png) |
|-|

**Exporting our trail data**
| ![exportlewisburgsaddle](images/inat-trail-exporting-path.png) |
|-|

Rename your exported file to reflect the trail and move it into the examples directory (e.g. `~/iNat-trails-master/examples/`). Now, lets rerun with our trail!

To search for all taxa, simply run default parameters: 
`./inat_trails.py examples/lewisburgsaddle.gpx`

To search for specific taxa, add a `--iconic_taxon` flag. For example: 
`./inat_trails.py --iconic_taxon Fungi examples/lewisburgsaddle.gpx`

Additional parameters can be found by running `./inat_trails.py -h`

Let's check out the output: 
* Species Table: `./McDonald_State_Forest_all_research_observations.html`
* Map of Species Along the Trail: `./McDonald_State_Forest_all_research_mapped_observations.html`

**The trail species map output file**
| ![speciesmap](images/inat-trail-species-map-prefilter.png) |
|-|

**The species list output file**
| ![specieslist](images/inat-trail-list-output.png) |
|-|

On both the map and the species list you can click on waypoints and links, respectively, to get more information about each observation. If you expected more observations along your trail it's probably because the default observation filtering is `research-grade`. Run `./inat_trails.py -h` to explore observation grade options amongst other customizable parameters. 

```
usage: inat_trails.py [-h] [--quality_grade QUALITY_GRADE] [--iconic_taxon ICONIC_TAXON] [--login_names] gpx_file [gpx_file ...]

positional arguments:
  gpx_file              Load GPS track from .gpx file.

optional arguments:
  -h, --help            show this help message and exit
  --quality_grade QUALITY_GRADE
                        Observation quality-grade, values: all, casual, needs_id, research; default research.
  --iconic_taxon ICONIC_TAXON
                        Iconic taxon, values: all, Actinopterygii, Amphibia, Animalia, Arachnida, Aves, Chromista,
                        Fungi, Insecta, Mammalia, Mollusca, Plantae, Protozoa, Reptilia; default all.
  --login_names         Show login name instead of numeric observation id in table of observations.
  --month               Show only observations from this month and the previous and next months.
  ```


---

## Exporting photos
[Back to top](#table-of-contents)

* [iNaturalist Open Data](https://github.com/inaturalist/inaturalist-open-data)



---

## iNaturalist publications overtime
[Back to top](#table-of-contents)

Scrape Google Scholar to count iNaturalist publications between inception (2008) and today. Python script [here](https://github.com/Pold87/academic-keyword-occurrence). 

To run: `python extract_occurrences.py 'iNaturalist' 2008 2022`

The output will print to your screen and you can either save it directly to a new file by copy+paste or using a redirect (>).

Make a dataframe with the output either manually (as I have) or by importing your table (load the `readr` library and then run `read.csv([yourfilepath.csv`])).

```r, iNaturalist publications overtime
library(ggdark)

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

## Resources
[Back to top](#table-of-contents)

* [Caltopo](https://caltopo.com/)


---
## Contact?  
[Back to top](#table-of-contents)

Feel free to [email me](mailto:michele.wiseman@oregonstate.edu?subject=Mapping%20iNaturalist%20Observations) with questions, comments, or if you're interested in collaboration. Follow what I'm up to on my [website](https://mswiseman.github.io/), [twitter](https://twitter.com/mswiseman), and/or [iNat](https://www.inaturalist.org/people/mswiseman) page. 

---

## Acknowledgments
[Back to top](#table-of-contents)

I'd like to thank my PhD advisor (and often, general life mentor) **David Gent** who is endlessly supportive of eccentric projects and is incredibly patient, kind, and brilliantly insightful. I feel so lucky to be advised by you. I'd also like to thank my **Jessie Uehling**, **Dan Luoma**, and **Joey Spatafora** for keeping me in the fungal loop inspite of having strayed to the plant side for my dissertation work. Jessie has been especially encouraging of all my mapping pursuits. Thank you to **Ishika Kumbhakar** who is absolutely brilliant with GIS data and mapping and often comes in clutch when I'm troubleshooting difficult analyses. Finally, thank you to my close network of friends and family members who are so patient with my neverending endeavors: my mom (Judi Sanders), twin sister (Nicole Wiseman), brother (Michael Wiseman), and the super amazing mildew mafia (Teddy Borland, Briana Richardson, Steve Massie, Nanci Adair, Carly Cooperider, Nadine Wade, Liz Lopez, Tatum Clark).
