---
title: Roman Emperors
author: Zac de Lusignan
date: '2020-11-08'
slug: roman-emperors
categories: ['Data Visualization']
tags: ['R']
description: ''
featured: ''
featuredalt: ''
featuredpath: ''
linktitle: ''
type: post
---

## Introduction

From TidyTuesday's Roman Emperors [data set](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-08-13).

![Chart](/img/main/emperors.png)

The chart shows us that elected emperors had longer reigns than those who rose to power by other means and the shortest reigns were by those who purchased their power. 

## Code

Below is the code used to create the visualization:

````
#Load the libraries
library(tidyverse)
library(tidytuesdayR)
library(ggthemes)

#Custom palette
romana = c("#8E1F2F","#702963","#B85C28","#297036", "#F0BC42","#26619C","#321C6F","#1F8E7E")

#Read in the dataset
emperors <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-08-13/emperors.csv")

#Convert the dates into numeric
emperors$reign_start <- as.numeric(emperors$reign_start)
emperors$reign_end <- as.numeric(emperors$reign_end)

#Find the mean reign for each method of assuming the throne
reign <- emperors %>%
  group_by(rise) %>%
  mutate(reign_length = reign_end - reign_start) %>%
  summarize(mean_reign = mean(reign_length)) %>%
  mutate(years_in_power = mean_reign / 365)

#Create a plot
p <- ggplot(reign, aes(x = years_in_power, y = reorder(rise, years_in_power))) +
  geom_bar(aes(fill = rise), stat = "identity") +
  scale_fill_manual(values = romana) +
  scale_x_continuous(breaks = seq(0, 12, 2)) +
  labs(title = "Roman Emperors",
       subtitle = "Rise to Power and Length of Reign",
       caption = "Data Source: Wikipedia",
       x = "Average Length of Reign (years)",
       y = "") +
  theme_solarized_2()

#Font adjustments
p + theme(legend.position = "none",
          plot.title = element_text(family = "Trajan Pro",
                                    face = "bold",
                                    color = "black",
                                    size = 24),
          plot.subtitle = element_text(family = "Trajan Pro",
                                       color = "black"),
          axis.title.x = element_text(color = "black"),
          plot.caption = element_text(color = "black"),
          text = element_text(family = "Trajan Pro",
                              color = "black")
          )
````