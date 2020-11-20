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

The chart shows us that elected emperors had longer reigns than those who rose to power by other means and the shortest reigns were by those who purchased the throne.

## Code

Below is the code used to create the visualization using R:

````
#Load the libraries
library(tidyverse)
library(tidytuesdayR)
library(ggthemes)

#Custom color palette
imperiumromanum = c("#8E1F2F","#702963","#B85C28","#297036", "#F0BC42","#26619C","#321C6F","#1F8E7E")

#Load the dataset
emperors <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-08-13/emperors.csv")

#Convert the dates into numeric
emperors$reign_start <- as.numeric(emperors$reign_start)
emperors$reign_end <- as.numeric(emperors$reign_end)

#Find the mean length of reign for each method of assuming the throne
reign <- emperors %>%
  group_by(rise) %>%
  mutate(reign_length = reign_end - reign_start) %>%
  summarize(mean_reign = mean(reign_length)) %>%
  mutate(years_in_power = mean_reign / 365)

#Create a plot
p <- ggplot(reign, aes(x = years_in_power, y = reorder(rise, years_in_power))) +
  geom_bar(aes(fill = rise), stat = "identity") +
  scale_fill_manual(values = imperiumromanum) +
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

Note: I was curious why those who purchased their power had such short reigns and decided to do further research. In 193 CE, Didius Julianus paid the Praetorian Guard 25,000 sesterces (this was 41 times a soldier's annual salary) for every soldier in the army in exchange for being named emperor. He was widely despised for this as the people found such blatant corruption insulting and proceeded to mock him whenever he appeared in public. After nine short weeks as emperor, the senate ordered his execution.
