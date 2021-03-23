---
title: "B_dendrobatidis"
output: github_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
------------------------------------------------------------------------

# Data

Load the data onto your environment from a **GBIF** download.

```{r Loading Data, echo=FALSE}
library(readr)
Bdend_records <- read_delim("~/ChytridFungus/0201192-200613084148143/0201192-200613084148143.csv", 
    "\t", escape_double = FALSE, trim_ws = TRUE)
```

```{r Create Table for Data}
library(tidyverse)
library(kableExtra)
kable(head(Bdend_records), format = "html", caption = "*Batrachochytrium dendrobatidis* records head from **GBIF** (only Australia)") %>%
  kable_styling() %>%
  scroll_box(width = "100%", height = "100%")
```

------------------------------------------------------------------------

## Important Data

<br> Let's take a closer look at the dataset downloaded from **GBIF**. <br>

#### Location Data

These are the most useful data in the current state of these records.

```{r Location Data, echo=FALSE}
location_data <- select(Bdend_records, c("decimalLongitude", "decimalLatitude"))
kable(head(location_data), format = "html", caption = "Latitude and Longitude data for *B. dendrobatidis*")
```

------------------------------------------------------------------------

## Initial Mapping

```{r Load Mapping Packages}
library(ggmap)
library(maps)
```

```{r Australian Map}
aus_map <- get_stamenmap(bbox = c(left = 113.338953078,
                                    bottom = -43.6345972634,
                                    right = 153.569469029,
                                    top = -10.6681857235),
          maptype = "toner-background", 
          crop = FALSE,
          zoom = 7,
          color = "bw")

```


```{r Occurrences}
ggmap(aus_map) +
  geom_point(aes(x = decimalLongitude, y = decimalLatitude), data = Bdend_records, alpha = .5, color="darkred", size = 2)
```
Here are the occurrence records shown throughout Australia. The opacity of the
occurrence data (shown in red) contributes to visualizing density the density
of species occurrences as the opacity increases.



```{r year}
year <- Bdend_records %>%
  select("year")
year <- na.omit(year)
```
***
# Findings

```{r}
ggplot(Bdend_records, aes(x = decimalLongitude, y = decimalLatitude)) + 
  geom_point() + 
  coord_equal() + 
  xlab('Longitude') + 
  ylab('Latitude')
```

```{r}
library(spatstat)

```
```{r}
Q <- quadratcount(location_data, nx = 6, ny = 3)
```

#### Density Estimate

```{r}
ggplot(Bdend_records, aes(x = decimalLongitude, y = decimalLatitude)) + 
  geom_point() + 
  coord_equal() + 
  xlab('Longitude') + 
  ylab('Latitude') + 
  stat_density2d(aes(fill = ..level..), alpha = .5,
                 geom = "polygon", data = Bdend_records) + 
  scale_fill_viridis_c() + 
  theme(legend.position = 'right')
```





```{r}
province <- select(Bdend_records, stateProvince)
province
```

## Year Distributio

```{r Year Distribution}
sort(unique(year$year))
```

```{r}
exbind <- bind_cols(s31 <- count(year, year == "1931"),
s78 <- count(year, year == "1978"),
s83 <- count(year, year == "1983"),
s85 <- count(year, year == "1985"),
s86 <- count(year, year == "1986"),
s89 <- count(year, year == "1989"),
s90 <- count(year, year == "1990"),
s91 <- count(year, year == "1991"),
s92 <- count(year, year == "1992"),
s93 <- count(year, year == "1993"),
s94 <- count(year, year == "1994"),
s95 <- count(year, year == "1995"),
s96 <- count(year, year == "1996"),
s97 <- count(year, year == "1997"),
s98 <- count(year, year == "1998"),
s99 <- count(year, year == "1999"),
s00 <- count(year, year == "2000"),
s01 <- count(year, year == "2001"),
s02 <- count(year, year == "2002"),
s03 <- count(year, year == "2003"),
s04 <- count(year, year == "2004"),
s05 <- count(year, year == "2005"),
s06 <- count(year, year == "2006"),
s07 <- count(year, year == "2007"),
s08 <- count(year, year == "2008"),
s09 <- count(year, year == "2009"),
s10 <- count(year, year == "2010"),
s12 <- count(year, year == "2012"))
```

```{r}
exbind
```
