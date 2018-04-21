# Data-science-at-scale-Data-viz-project

Please click the other .md file to see my code and output for this project.

This data visualisation project looks at crime data from Seattle and San Francisco from summer 2014. I decided to compare crime in the two cities, because it was suggested that this would be difficult since the variables in the two cities datasets did not have the same names. It required some exploratory data analysis to determine the layout and form of the data. The crimes are labelled differently in the two cities; the data analyst would need input from a subject matter expert if comparing crimes in the two cities.

I am very pleased to have plotted where the crimes occurred on maps in R. They are not pretty visualisations but this is only a project for my own interest. I'm happy I managed to do something with a map!  

In another graph, I aggregated 92 days of data and plotted one point for the number of crimes that occurred in each hour of the 24 hour period. I did this for both cities. It is quite clear that the number of crimes per hour varies over the 24 hours. I wondered whether the pattern (more crime during daylight hours, lowest number of crimes in the early morning, and so on) was the same for both cities. If the pattern is the same for both cities, the "expected" number of crimes can be calculated as the proportion of the total number of crimes committed that hour in both cities split by the total number of crimes committed in both cities over the whole period. This is what the expected values are on the plots. A chi square test is what would be used to assess whether the pattern of crime was the same over the hours for the two cities. (As a first effort. This data could more correctly be considered a time series, but I'm just going to do something simple. I'm still feeling quite pleased I got the maps to work!)

A couple of thoughts on what else I'll do on this project: 

1. There are excess numbers of crimes committed in the midday hour and the midnight hour. I will investigate if these are associated with particular types of crimes. Maybe it will be easy to see that these are crimes such as theft from a home, where it is unsure what time the crime occurred so midday has been allocated. 

2. Approx 2000 lat and long values are missing from one dataset. I'll investigate whether these are associated with a particular time of day or a particular type of crime.
