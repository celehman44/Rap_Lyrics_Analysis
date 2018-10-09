### Problem statement
For my capstone, I plan on analyzing rap lyrics from the years 1995 - 2018. Through my analysis, I hope to answer the question, Can we predict a song's appearance on the weekly billboard 100's using only its lyrics? Rap music is more lyric centric than other
types of music, so I'm hoping that only using lyrics is a plausible way to predict a songs appearance on the billboard 100's. I also think it will be interesting to look at how the topics and lyrics change as a function of time. I plan on collecting my song lyrics from genius using it's api, and I plan on getting the billboard data by utilizing the billboard api wrapper. I expect my data to be about 15,000 - 25,000 rows, and several hundreds to thousands of columns.

### The Data
The data, which consists of songs, has features for lyrics, the artist, year released, and album. All data
was sourced from Genius, by way of an API wrapper and around 34,000 songs were scraped.


### Progress Report
I currently have full data in hand, but I am struggling to decide whether or
not I should drop rows that have a missing year or to impute them.
I have not done a full EDA, or really any EDA for that matter but plan on
completing all my EDA and plots by the end of this week. Processing power is
a slight issue, as it takes longer
than I would like to run operations on my data frame
(I will need to raise the volume on my AWS). My data frame has about 34000 rows
and 4 columns (this is before tokenizing my lyrics). After completing EDA
this week, next week I would like to focus in on modeling. 
