### Problem statement
For my capstone, I plan on analyzing rap lyrics from the years 1995 - 2018. Through my analysis, I hope to answer the question, Can we predict a song's appearance on the weekly billboard 100's using only its lyrics? Rap music is more lyric centric than other
types of music, so I'm hoping that only using lyrics is a plausible way to predict a songs appearance on the billboard 100's. I also think it will be interesting to look at how the topics and lyrics change as a function of time. I plan on collecting my song lyrics from genius using it's api, and I expect my data to be about 15,000 - 25,000 rows, and several hundreds to thousands of columns.

### The Data
The data, which consists of songs, has features for lyrics, the artist, year released, and album. All data
was sourced from Genius, by way of an API wrapper and around 34,000 songs were scraped.


# Technical Report
In my Capstone, I decided to answer the question, Can we predict a song's appearance on the billboard hot 100's based on its lyrics? To answer this question, I had to scraped over $30,000$ (even though only $17,000$ ended up being used) songs from genius.com using an api wrapper sourced from this github: https://github.com/johnwmillr/LyricsGenius
After scraping the songs and tokenizing the data and decomposing it, I fit several models, and my best score ended up being from a logistic regression model with an accuracy score of $70\%$ (on balanced classes, so baseline is $50\%$).

## The Data
The integrity of my data was crucial to my model's success, so being extra prudent certainly paid off here. In order to use the api wrapper lyrics genius, I needed a list of rappers names to feed it, so initially I scraped Ranker.com for a list of rappers names, and I had to use selenium to click a load more button to exhaustion. In the end I scraped the entire list and got about 700 names. If you want to see this process, check out notebook 1, titled Scraping_Rapper's_names.

The next stop in the process was to feed this list to the genius api wrapper lryics genius to collect each artist's entire library to a json. This ended up taking about 5 days, and in the end made me realize I should have just used requests instead of a wrapper. Check out notebook 2, Scraping_lyrics to see this in action.

After collecting the json's I constructed a dataframe consisting of the lyrics, artist, date, and song title for each song. It looks something like this:

![Screenshot](./images/scr_shot_1.jpg?raw=true "Title")

This happened in notebook 3.

As you can see, it's fairly messy, with things like [intro:], or [verse1:] scattered throughout. There are also newline characters and lots of punctuation that needed to be taken care of in order for tokenization to go smoothly. All of this got dealt with in notebook 4.

In notebook 5, I created my target variable that encoded whether or not the song charted the billboard hot 100's. This required sourcing billboard chart data from online. I had to grab two different billboard datasets because one ended at 2014. Here is what those two datasets look like:

![Screenshot](./images/scr_shot_2.jpg?raw=true "Title")

![Screenshot](./images/scr_shot_3.jpg?raw=true "Title")

As you can see, they are quite different. I had to merge those two billboard datasets onto my lyrics dataset shown earlier in order to determine which artists and their songs appeared in the billboard database. I ended up with about 830 hits out 25,000 songs. This occurred in notebook 5. In notebook 6, I manually filled in the missing data for my positive class, and dropped the rest of my data that had missing lyrics or years (I didn't have time to fill in all the missing data and didn't think imputing the data was the answer). This cut my negative class to about $17,000$, which is the amount of songs I ended up modeling on. Here is what the final dataset looked like:

![Screenshot](./images/scr_shot_9.jpg?raw=true "Title")

## Tokenizing
In order to model on lyrics, I had to create a dataframe where each column was a word, each row was a song, and each cell would be either the count of the word in that song (count vectorizing) or the frequency of that word in that song relative to the frequency of that word in the document (term frequency inverse document frequency). Before I did this, I had to lemmatize the data (for example, cutting off the 'ing' or 'in' from words), so there wasn't redundancy in the columns. Because there was so much slang, I had to create a lot of custom lemmatization patterns as well as custom stop words (words to not be included). In the end, TFIDF performed much better, so I used it for modeling, but I used count vectorization for modeling. Tokenizing occurred in notebook 7

## Modeling
Because my classes were so unbalanced, I ended up bootstrapping (random sampling with replacement) my data after I split it up into train and test datasets (so I can see how my model scores on unseen data). My test dataset ended up having $4,000$ in the positive class and 4,000 in the negative class.

Because there were so many columns due to tokenizing, I decided to use singular value decomposition to reduce the amount of features to about 100. SVD creates its components by finding the features that explain the maximum amount of variance iteratively. It ended up scoring best and being less overfit.
For modeling I attempted several models, such as random forest, ada boot, gradient boost, and support vector machines, but the best performer ended up being logistic regression with score of $71\%$. Here's its confusion matrix below:

![Screenshot](./images/scr_shot_4.jpg?raw=true "Title")

I ended up predicting 2636 out of 4000 positive observations correctly. So my true positive rate is about $66\%$. My false negative rate (which we would want to minimize, because it means we missed spotting a hit) was only about 34\%.

## EDA
A huge component of my project was exploring rap lyrics and trying to find patterns in it over time, such as this plot:

![Screenshot](./images/scr_shot_5.jpg?raw=true "Title")

Here we can see that over time the number of hits per year increases ver steadily. This could be due to a number of factors, such as raps growing popularity and or the growing number of rap artists.

It was also found that the most common words in rap music were all cuss words.

![Screenshot](./images/scr_shot_6.jpg?raw=true "Title")

In particular, the N word is used about twice as much as the next top word, the F word.

I also plotted several scatter plots of words that had high weights in my top component.

![Screenshot](./images/scr_shot_7.jpg?raw=true "Title")

This plot shows that the word girl appeared in hits more than they did in non-hits, and that the word is decreasing in popularity over time. Arguably the most important word (it had the highest weight in the most important component) when determining a hit, is money.

![Screenshot](./images/scr_shot_8.jpg?raw=true "Title")

Recent hits have more occurrences of the word money that non-hits, however, before 2002 the opposite was true. Also in general the word is increasing in popularity over time.

Here is a bar plot showing artists with the most amount of

![Screenshot](./images/scr_shot_10.jpg?raw=true "Title")

 As we can see Drake, Kanye West, and T.I. have the most hits, unsurprisingly.

 Let's see which artist has the largest vocabulary

 ![Screenshot](./images/scr_shot_11jpg?raw=true "Title")

... As well as the worst vocabulary

![Screenshot](./images/scr_shot_12jpg?raw=true "Title")

If you want to see more plots, I recommend you check out notebook 10.

## Conclusion
In the end, I believe that 
