# Ultra_Phase1v5_Phase2v2


-----------------------------------------------------------------------------
## Objective of this Project:

**The objective of this project is to practice some big-data analysis, like topic trending, user behavior profiling and sentiment analysis on text messages.** 

The case which was chosen to study is the 2016 presidential election, since this is certainly a major media and public focus point. Twitter was chosen as the data source, primarily due to its free twitter API. 


-----------------------------------------------------------------------------
## How this Objective is Pursued:

This project is currently divided into three phases, each for distinct purposes. Please check session "Introduction to the DataSet of this Project" below for information regarding to data set used for this project.

**Phase1 of this project is devoted to data cleaning and extracting basic relevent information from data set**

The Phase1v5 is the 5th re-write of Phase1. Since we have the 2016 election as our focus point, without caring too much about irrelevent topics, thus only a small part of the raw data set are relevent to our purposes. In order to extract these relevent data, I adopted a dynamic method for data cleaning. The basic idea is that, during the process of scaning through the data set, one will build up a dictionary for hash tags, keywords and individuals that are more closely related to the 2016 elections(tags and individuals have different estimation standards). And we are using this dictionary to extract relevent information. 

**Phase2 of this project is devoted to sentiment analysis**

During my previous attempts at this subject, I realized that purely statistical variables, though abundent, is not very effective when it comes to figuring out individual's opinion, nor when it comes to classify what kinds of information certain specific individual is receiving. In short, sentiment analysis of tweet text message is absolutely necessary for achieveing the objective. 

The sentiment analysis in Phase2v2 (2nd re-write of Phase2) is performed using the LSTM (Long Short Time Memory) method, written with Theano. Programing implementation wise, this is not particularly challenging. However, what is challenging is how one generates the corpus to train the LSTM network. 

Usually, the "golden standard" corpus when it comes to training a NLP application is a corpus that is manually marked by human beings. Naturally, I don't have time to marked hundreds of thousands of tweets. But the good news is that, since we are studying tweet messages, they all come with one or more hash tags. Thus, I could mark the most frequently used hashtags, and use these hashtags to mark the opinions expressed by tweet messages. Nonetheless, there are still several challenges related to generating a corpus for training. 

1st, of those highly used hash tags, some are neutral, like "trump2016" or "hillary2016". These hash tags are not necessarily used to express support or dislike. Thus one could not use them to mark tweet messages. 

2nd, among hash tags that are clearly biased towards certain sentiments, like "vote4XXXX" or "XXXXislier", those hash tags that express dislike are much more frequently used than those express supported. As a results, the corpus one could get in this way is heavily biased towards the "dislike" sentiment. For both candidates (trump and hillary), the number of tweets expressing dislike is easily ten times larger than that of support. 

**Phase3 of this project will be built on Phase1 and Phase2 to generate interesting results**

With data base generated by Phase1, one could easily pull some interesting results that are purely statistical, like ranking of most popular hash tags, more active users, etc. However, it is those purely statistical results combined with Sentiment Analysis from Phase2 that would give the most powerful, meanful results.


-----------------------------------------------------------------------------
## Results Demonstration:  

### Preliminary Statistical Results on Hash Tags

The two pictures below are generated using a 18-days' continous data set took in July 2016. It demonstrates, on a day-to-day basis, the top 10 most used tags' relevence to the two candidates (Trump and Hillary) as well as how the "most used tags in a specific day" fares when one stretches the time window into three weeks. 

The number before each hash tag are the number of its usage during the day which is specified by the date displayed at top middle title. 

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/tag_july_relevence.gif)

Relevence of each hash tag to a candidate is scaled as: if the hash tag contains keyword "trump"( or "hillary"), then its relevence is set to 10 correspondingly; if the hash tag doesn't contain any keyword but is ALWAYS used with hash tag of relevence 10, then its relevence would stay at 9; the less frequently a hash tag is associated with keyword or hash tags of higher relevence, the lower its relevence score.

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/tag_july_HisCall.gif)

Here, the "cumulated number of call" is the cumulated number of a hash tag's usage during the 18-day time window. Naturally, this cumulated number will increase as one moves to a later date. For example, one could clearly see that #demsinphilly is a really hot topic between July 25th and July 29th. 

The two pictures below are generated using 25 days' worth of data (18 days in July, 7 days in October). It demonstrates, for a selected few "neutral, high usage" hash tags, how their usage as well as the sentiment they carry change over time. 

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/trump_tags.gif)

The bar plots gives the usage per day, while the red and blue lines gives the "sentiment" they carry. For example, if the sentiment value towards trump (red line) is really close to +1, then it means most of tweets that called this hash tag is expressing support for candidate Trump. In my analysis, each hash tag is assigned two sentiment values, one for each candidate. 

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/hillary_tags.gif)

From the above two pictures, one could observe that Trump's most used hash tags generally are supportive. Particularly, both #trumppence16 (this tag is more closely related to Trump's campaign) and #trump (this tag is used much more widely, including discussion about candidate Trump himself) demonstrates increasing approval scores in the leading up to November. However, the story for candidate Hillary is the opposite. With the exception of #hillary2016 (a tag more closely related to Hillary's campaign), which is barely above 0 even for the blue line, all other tags all below 0. 

An explanation about how I calculated those "neutral" hash tags' sentiment score would be necessary. I did it **using a dynamic, iteration method:**  
 1. I manually marked a few hash tags that carries a very clear sentiment. [Marked Tags](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/tree/master/Ultra_Phase3v1/Data). 
 2. I noticed that people tends to call multiple hash tags at once. Thus, it would be possible to build up a dictionary for each hash tag, recording: what other hash tags are used together, what is the number of usage of those "used together" hash tags. These work are done in Phase1. 
 3. I could calculate sentiment scores of any specific neutral hashtag using "clearly biased" hash tags that are inside its dictionary, through iteration, on a day-to-day basis. 

### Most Used Hash Tags on Nov 6th

In the above results estimating sentiment of specific tags towards particular candidate, scores of those hand-marked tags are kept constant at the hand-marked value. However, I have noticed that some users may call hash tags related to both candidates while expressing clear support for one of the candidate in his/her tweet messages. Thus, I decided to relax the sentiment scores for those hand-marked hash tags, only assigning them the hand-marked value at the beginning of each day's iteration, then allowing their scores to float according to the context. The result is shown below. The hash tags are the top 30 most used tags on Nov 6th, including both hand-marked tags and "neutral" tags. 

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/Nov6th_TopTags.gif)

As one could see: 
- # trump toke the daily most used hash tag position. # trump has consistantly been the most used hash tag through out the election campaign. 
- # imwithher toke the second place. This is hardly a surprise, as this tag is closely related to Hillary's campaign, and should be highly used at this terminal stage of the election campaign. However, # imwithher's usage is no where close to # trump. Worse still, its sentiment score towards Hillary is barely above zero. This is a surprise
- # maga follows in the 3rd place. MAGA (short for Make America Great Again) is closely related to Trump's campaign. Different from # imwithher, the sentiment score is clearly positive (negative) towards Trump (Hillary).
- # hillary is in the 9th place. As with the case of # trump, this tag is more related to discussions about Hillary herself rather than her campaign. In the leading up to election day, the sentiment score towards Hillary is improving, but still negative, indicating considerable proportion of users hold a negative opinion towards Hillary
- # vote is a hash tag that saw increasing use only when we approaches the election day. This hash tag should be bipartisan, and both its sentiment scores are indeed positive and close to zero. Still, we saw a lead of Trump here. 


-----------------------------------------------------------------------------
## Introduction to the DataSet of this Project:

In order to achieved such objective, one has to download and analyze huge amounts of social media data. The reason that one needs such a large, continuous data set is because: 1st, since we are trying to understand individual's shifting opinions, we naturally what to have a continuous data set so as not to "miss something"; 2nd, since the fact that individual usually takes some time to have his/her mind changed, and that most people don't spend all day on twitter, thus each individual's activity is rather sparse in time, thus one need to collect data for a reasonably long period of time.

In this case, I have so far collected (commented on Nov 5th, 2016) 28 days' worth of data, among which 18 contineous days in July (July 13th to Aug 2nd, 2016), 7 contineous days in October (Oct 15th to 21st, 2016) and attempting a contineous data set from Oct 31st til some time after Nov8th election day. On average, each day's worth of data is ~7GB with 3 million tweets. I am only collecting tweets with geo-information indicating its user located within the United States. 

It is worth noting that 3 million tweets per day is apparently too few for such a user base. My guess is that the amount of tweets I could collect is limited by my twitter API query speed. However, since tweets are downloaded randomly (those that are got/missed by my API), it won't have any negtive effects on my analysis. 


-----------------------------------------------------------------------------
## The Way Forward

This deposite will be routinely updated for the next one month or so (until I hit a dead end). 

What I intend to do next, is dynamically expand the set of hash tags that I could use for marking tweet messages. 

Also, I will try a "boost with different LSTM as contributing elements". 

-----------------------------------------------------------------------------
## Technical Details

This set of codes are written in Python 2.7, using libraries that includes: theano, pymysq and other routine libs like numpy and pandas.

Phase1v5 and Phase2v2 both demands MySQL as the way of managing huge data sets. 

The tokenization scripts are borrowed from https://github.com/moses-smt/mosesdecoder/raw/master/scripts/tokenizer/tokenizer.perl. Credits to Josh Schroeder. 

The LSTM training and predictions codes are adapted from the tutorial codes at http://deeplearning.net/tutorial/contents.html. I adapted the training codes so that it would work on my data sets; I added prediction codes so that it would work with other parts of my Phase2v2. 

