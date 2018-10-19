### Problem statement
For my capstone, I plan on analyzing rap lyrics from the years 1995 - 2018. Through my analysis, I hope to answer the question, Can we predict a song's appearance on the weekly billboard 100's using only its lyrics? Rap music is more lyric centric than other
types of music, so I'm hoping that only using lyrics is a plausible way to predict a songs appearance on the billboard 100's. I also think it will be interesting to look at how the topics and lyrics change as a function of time. I plan on collecting my song lyrics from genius using it's api, and I expect my data to be about 15,000 - 25,000 rows, and several hundreds to thousands of columns.

### The Data
The data, which consists of songs, has features for lyrics, the artist, year released, and album. All data
was sourced from Genius, by way of an API wrapper and around 34,000 songs were scraped.


### Technical Report
In my Capstone, I decided to answer the question, Can we predict a song's appearance on the billboard hot 100's based on its lyrics? To answer this question, I had to scraped over $30,000$ (even though only $17,000$ ended up being used) songs from genius.com using an api wrapper sourced from this github: https://github.com/johnwmillr/LyricsGenius
After scraping the songs and tokenizing the data and decomposing it, I fit several models, and my best score ended up being from a logistic regression model with an accuracy score of $70\%$ (on balanced classes, so baseline is $50\%$).

## The Data
The integrity of my data was crucial to my model's success, so being extra prudent certainly paid off here. In order to use the api wrapper lyrics genius, I needed a list of rappers names to feed it, so initially I scraped Ranker.com for a list of rappers names, and I had to use selenium to click a load more button to exhaustion. In the end I scraped the entire list and got about $700$ names. If you want to see this process, check out notebook 1, titled Scraping_Rapper's_names.

The next stop in the process was to feed this list to the genius api wrapper lryics genius to collect each artist's entire library to a json. This ended up taking about 5 days, and in the end made me realize I should have just used requests instead of a wrapper. Check out notebook 2, Scraping_lyrics to see this in action.

After collecting the json's I constructed a dataframe consisting of the lyrics for each song 

![My image](celehman44.github.com/Capstone/images/scr_shot_1.jpg)
