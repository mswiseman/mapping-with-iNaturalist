# Code for graphs used in my talk

* [Code for Counting and Visualizing Papers Discussing iNaturalist](#iNaturalist-publications-overtime)
* 

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

I added the text and arrow in powerpoint, but you can easily add it in ggplot using `geom_text` or `geom_label`. 

![iNaturalist citation growth](images/iNatPaperGrowth.png)
