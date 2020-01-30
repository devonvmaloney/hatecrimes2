# hatecrimes2
 Attempt2

---
title: "A Geovisualization of Hate Crimes in the US from 1991-2017"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## R Markdown
Group Project 1
Geog 456 Geovisualizing change
Devon Maloney 
1/29/2020

To set up the data, I seperated the national individual records of data and place into counts of incidents per month in excell by using the COUNTIF function for all unique month + year combinations. I did this again for just the New Jersery data, a state with a more complete record of hate crime data collection. 

## Packages and Directory

```{r pressure, echo=FALSE}
#install.packages("ggplot2") 
#install.packages("RColorBrewer")
#install.packages("ggridges")

library(ggplot2)
library(RColorBrewer) 
library(ggridges)
library(readr)

setwd ("~/Documents/UNC/Spring 2020/456 Geovisualizing Change/HateCrime")

hatedata <- read_csv("monthyear_indicident.csv")
hatedataNJ <- read_csv("monthyear_indicidentNJ.csv")

```

Look at the data 
```{r}
summary(hatedata)
summary (hatedataNJ)
```


## Bar graphs for seasonality 
Make a bar graph with a title, axis labels and caption with simple ggplot bar graph. 

```{r}
p = ggplot(hatedata, aes(x=Month, y = incident)) + geom_bar(stat="identity") 
pnj = ggplot(hatedataNJ, aes(x=Month, y = incident)) + geom_bar(stat="identity") 

#Add labels
p = p + labs(title="Seaonality of hate crimes", x ="Month", y = "Number of indicdents", subtitle = "National rates", caption = "(based on data from the FBI)") 
p 

```

Repeat with New Jersey data: 

```{r}

pnj = pnj + labs(title="Seaonality of hate crimes", x ="Month", y = "Number of indicdents", subtitle = "New Jersey rates", caption = "(based on data from the FBI)") 
pnj 
```

## Warming stripes
Now make the monthly graphs into a warming stripe graph.

```{r}
# National  
# Colors 
p = p + aes(fill = incident) # assign gradient of blue color to the temp value 
p = ggplot(hatedata, aes(x=Monthyear, y = incident, fill = incident)) + geom_bar(stat="identity")

# Look at the brewer color library and select a nice color gradient. We will select "RdYlBlu". 
display.brewer.all() 
p + scale_fill_gradientn(colours = colorRampPalette(brewer.pal(11,"RdYlBu"))(11)) # red to blue  
p <- p + scale_fill_gradientn(colours = colorRampPalette(rev(brewer.pal(11,"RdYlBu")))(11)) # REV to adjust scale from blue to red 
p
p + aes(y = 300) # make it warming stripes
p + aes(x = as.factor(Monthyear), y = 300) # to avoid missing stripes, make yearmonth a factor 

# Add add minial theme, legend, titles, axis labels. 
# Ideally, this section would also include a way to label the year on the x axis, but I was unable to do that. 
p + aes(x = as.factor(Monthyear), y = 300) + theme_minimal() + theme(axis.text = element_blank(), axis.title = element_text(), panel.grid.major = element_blank(), panel.grid.minor = element_blank(), legend.position='right') + labs (x= "Month", y = "Incidents", title = "National Incidents of Hate Crime", subtitle = "Monthly from 1991 - 2017", caption = "(based on data from the FBI)")


```

Repeat process for New Jersey data: 

```{r}
# Colors 
pnj = pnj + aes(fill = incident) # assign gradient of blue color to the temp value 
pnj = ggplot(hatedataNJ, aes(x=Monthyear, y = incident, fill = incident)) + geom_bar(stat="identity")
pnj + scale_fill_gradientn(colours = colorRampPalette(brewer.pal(11,"RdYlBu"))(11)) # red to blue  
pnj <- pnj + scale_fill_gradientn(colours = colorRampPalette(rev(brewer.pal(11,"RdYlBu")))(11)) # REV to adjust scale from blue to red 
pnj + aes(y = 300) # make it warming stripes
pnj + aes(x = as.factor(Monthyear), y = 300) # to avoid missing stripes, make yearmonth a factor 

# Add add minial theme, legend, titles, axis labels. 
# Ideally, this section would also include a way to label the year on the x axis, but I was unable to do that. 
pnj + aes(x = as.factor(Monthyear), y = 300) + theme_minimal() + theme(axis.text = element_blank(), axis.title = element_text(), panel.grid.major = element_blank(), panel.grid.minor = element_blank(), legend.position='right') + labs (x= "Month", y = "Incidents", title = "New Jersey Incidents of Hate Crime", subtitle = "Monthly from 1991 - 2017", caption = "(based on data from the FBI)")
```


## Seasonality by year
Now make the warming stripes into petals. For this section, the independent variable is swtiched from Monthyear column to Month column. Add add minimal theme, legend, titles, axis labels. 

Issues: I tried to remove the unnessicary axises. Unknown why national graph is on 0.5 month increments and why the NJ is correctly on 1 month incriments. 


```{r}
# National  
# Colors 
p = p + aes(fill = incident) # assign gradient of blue color to the temp value 
p = ggplot(hatedata, aes(x=Month, y = incident, fill = incident)) + geom_bar(stat="identity")
p + scale_fill_gradientn(colours = colorRampPalette(brewer.pal(11,"RdYlBu"))(11)) # red to blue  
p <- p + scale_fill_gradientn(colours = colorRampPalette(rev(brewer.pal(11,"RdYlBu")))(11)) # REV to adjust scale from blue to red 
p
p + aes(y = 300) # make it warming stripes
p + aes(x = as.factor(Month), y = 300) # to avoid missing stripes, make month a factor 

#Add labels 
# Ideally, this section would also include a way to label the year on the x axis. 
p + aes(x = as.factor(Month), y = 300) + theme_minimal() + theme(axis.text = element_blank(), axis.title = element_text(), panel.grid.major = element_blank(), panel.grid.minor = element_blank(), legend.position='right') + labs (x= "Month", y = "Incidents", title = "National Incidents of Hate Crime", subtitle = "Monthly from 1991 - 2017", caption = "(based on data from the FBI)")

```

Repeat process for New Jersey data: 

```{r}
# Colors 
pnj = pnj + aes(fill = incident) # assign gradient of blue color to the temp value 
pnj = ggplot(hatedataNJ, aes(x=Month, y = incident, fill = incident)) + geom_bar(stat="identity")
pnj + scale_fill_gradientn(colours = colorRampPalette(brewer.pal(11,"RdYlBu"))(11)) # red to blue  
pnj <- pnj + scale_fill_gradientn(colours = colorRampPalette(rev(brewer.pal(11,"RdYlBu")))(11)) # REV to adjust scale from blue to red 
pnj + aes(y = 300) # make it warming stripes
pnj + aes(x = as.factor(Month), y = 300) # to avoid missing stripes, make month a factor 

# Add labels
pnj + aes(x = as.factor(Month), y = 300) + theme_minimal() + theme(axis.text = element_blank(), axis.title = element_text(), panel.grid.major = element_blank(), panel.grid.minor = element_blank(), legend.position='right') + labs (x= "Month", y = "Incidents", title = "New Jersey Incidents of Hate Crime", subtitle = "Monthly from 1991 - 2017", caption = "(based on data from the FBI)")


```

I'm not sure why the final aes() settings make the month ticks and month labels on the x-axis go away. This would be something I would like to include in the final graph. 

## Petal Circles
```{r}
#Make it into a petal circle 
p + coord_polar(theta = "x") 
pnj + coord_polar(theta = "x") 
```

Not sure why the national data had the months diplay in 0.5 month incriments and the NJ data appeared correctly as months. This is something I would like to resolve. 

All data from the FBI National Hate Crime Database. 

