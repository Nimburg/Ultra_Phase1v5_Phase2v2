# Project Ultra


-----------------------------------------------------------------------------
## Objective of this Project:

**The primary objective of this project is to study "How individual's opinion towards specific issue might shift under external influence".** What I have finished through this project includes some basic big-data analysis, like topic trending, user behavior profiling and sentiment analysis on text messages.

The case which was chosen to study is the 2016 presidential election, since this is certainly a major media and public focus point. Twitter was chosen as the data source, primarily due to its free twitter API. 

During my pursue of the primary objective, I also developed a "prototype" method for **Semi-Automatic sentiment analysis of social media text messages, without human-effort-generated training corpus**. The details of this method will be discussed in the **section "Ideas for Semi-Automatic Sentiment Analysis"** below. 


-----------------------------------------------------------------------------
## How this Objective is Pursued:

This project is currently divided into three phases, each for distinct purposes. Please check out **section "Structure of this Project"** below for information regarding how this project (codes) is structured. Please check out **section "Introduction to the DataSet of this Project"** below for information regarding to data set used for this project. 

### Phase1 of this project is devoted to data cleaning and extracting basic relevent information from data files

The Phase1v5 is the 5th re-write of Phase1. Since we have the 2016 election as our focus point, without caring too much about irrelevent topics, thus only a small part of the raw data set are relevent to our purposes. In order to extract these relevent data, I adopted a dynamic method for data cleaning. The basic idea is that, during the process of scanning through the dataset, one will build up a dictionary for hash tags, keywords and individuals that are more closely related to the 2016 elections(tags and individuals have different estimation standards). And we are using this dictionary to extract relevent information. 

### Phase2 of this project is devoted to sentiment analysis

During my previous attempts at this subject, I realized that purely statistical variables, though abundent, is not very effective when it comes to figuring out individual's opinion, nor when it comes to classify what kinds of information specific individual is receiving. For example, if we want to determine whether a specific user holds a positive or negative opinion toward a candidate, it is not enough to just check "whether this user called this candidate's name in his message". Instead, one needs to analyze "what this user said about this candidate in his message". In short, sentiment analysis of tweet text message is absolutely necessary for achieving the objective. 

There are two challenges here: the first one is how one generates the corpus to train the LSTM network; the second one is how one would build a model that is sensitive to rich expression method, like satire, irony and exaggeration. The details will be discussed in the **section "Ideas for Semi-Automatic Sentiment Analysis"** below. 

### Phase3 of this project will be built on Phase1 and Phase2 to generate interesting statistical results

With data base generated by Phase1, one could easily pull some interesting results that are purely statistical, like ranking of most popular hash tags, more active users, etc. However, it is those purely statistical results combined with Sentiment Analysis from Phase2 that would give the most powerful, meanful results.


-----------------------------------------------------------------------------
## Results Demonstration:  

### Preliminary Statistical Results on Daily Hash Tag Usage

The two pictures below are generated using a 18-days' continous data set took in July 2016. It demonstrates, on a day-to-day basis, the top 10 most used tags' relevence to the two candidates (Trump and Hillary) as well as how the "most used tags in a specific day" fares when one stretches the time window into three weeks. 

The number before each hash tag are the number of its usage during the day which is specified by the date displayed at top middle title. 

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/tag_july_relevence.gif)

Relevence of each hash tag to a candidate is scaled as: if the hash tag contains keyword "trump"( or "hillary"), then its relevence is set to 10 correspondingly; if the hash tag doesn't contain any keyword but is ALWAYS used with hash tag of relevence 10, then its relevence would stay at 9; the less frequently a hash tag is associated with keyword or hash tags of higher relevence, the lower its relevence score. Note that tags of similar relevences do not necessarily share the same sentiment. 

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/tag_july_HisCall.gif)

Here, the "cumulated number of call" is the cumulated number of a hash tag's usage during the 18-day time window. Naturally, this cumulated number will increase as one moves to a later date. For example, one could clearly see that #demsinphilly is a really hot topic between July 25th and July 29th. 

### Interesting Hash Tags used on the Election Day

First, let's look at hash tags that are directly related to the "voting", in the lead up to Nov 8th. Shown below are the four hash tags "vote", "electionday", "election2016" and "elections2016". One could see their daily usage got a dramatic increase towards Nov 9th(NOTE: as for why those hash tags only recorded a few thousands calls, even on the election day, please check the **section "Introduction to the DataSet of this Project"** below). All those neutral hash tags demonstrates a positive sentiment scores towards both candidates, indicating supporters on both sides are rallying for their heros. However, the sentiment score towards Trump has a clear lead, indicating that there is more active Trump supporters on twitter, and that less negtive information/hash tags related to Trump are called at this terminal stage.

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/ED_vote.gif)

Secondly, let's look at hash tags that are related to the FBI investigation into Candidate Hillary's email affair. Here I used hash tags "comey", "fbi" and "wikileaks", as these are the most frequently used during the last few days. As one could see, "comey" and "fbi" have a significant drop in their sentiment score toward Hillary around Nov 3rd, coinciding FBI's latest re-opening of the investigation. However, the sentiment score got a sharp increase on Nov 7~8th, coinciding not only the time when FBI clears Hillary's wrong doings, but also the election day when lots of supporters are rallying on social media. The hash tag "wikileaks" demonstrated a certain degree of correlation with the other two hash tags, and saw improving sentiment scores towards the election day after FBI clears Hillary's wrong doings round Nov 7th. 

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/ED_fbi.gif)

In conclusion, I am confident that my current method of estimating hash tags' sentiments is working. For details, please see **section "Ideas for Semi-Automatic Sentiment Analysis"** below. 


-----------------------------------------------------------------------------
## Ideas for Semi-Automatic Sentiment Analysis

In the context of social media, sentiment analysis occupies a central place when one tries to understand what is going on, since it is not enough to know "who talked with whom", rather, one needs to know "who talked what with whom". However, text messages from social media like twitter, reddit and factbook have some of its unique challenges. 

**The first challenge is of generating a training corpus.** Usually, the "golden standard" corpus when it comes to training a sentiment analysis application is a corpus that is manually marked by human beings. However, manually generated corpus takes human resources and money. **But the most critical short coming of such a manually generated corpuses is that it could not keep up with the flowing, dynamic context of social media.** As shown by the previous analyses, sentiments and context on a social media change from day to day, sometimes dramatically. On one hand, millions of text messages are generated daily; on the other hand, most social media based machine learing applications are intended for advertising, promoting and predicting. Thus, it is important to have a method to **rapidly generate a good enough training corpus roughly in real-time speed**.

**The second challenge is that text message on social media is usually very short.** Particularly for the case of twitter, where one could not lay down too much words, people's messages are usually very succinct. This means people are making references to many topics/keywords/hashtags of which their meanings and sentiments are already well konwn among twitter users. For the dataset I am using, among tweets that called election-related hashtags, only half would have more than 10 words that are not: other users' names, or http links to some web pages. But at the same time, **this challenge represents a unique opportunity**. Because people's expressions are so succinct, their languages are less descriptive, and more of what they want to express are allocated to those few keywords and hashtags. Thus, **the need to analyze structures of sentences or even paragraphs is less relevent than the need to figure out exactly what sentiments each of those keywords and hash tags carries**, ragarding to the topic under study (in this case, the 2016 election and users' opinions towards Trump and Hillary). An example is in order to elastrate the point we just made here. This message "for you non-stop! God Bless you for what youve endured these past 510 days, fighting for America **Salute** **MAGA**" was twitted out early in the night of Nov 8th, and it calls two hash tags #Salute and #MAGA. Without #MAGA, one could ONLY tell that this user expresses good wishes for his/her candidate. It is with #MAGA that we are certain this user is a supporter for candidate Trump. This is how much information could be centralized in a tweet, even when this tweet is already comparatively long. 

### Estimating Sentiments Carried by Hash Tags and Generating Corpus using Hash Tags

**What we are doing here, is to exploit the fact that we are studying tweet messages which mostly come with one or more hash tags.** Thus, one could hand-mark the sentiment of those most frequently used hashtags, and use these hashtags to get a rough estimation of the opinions expressed by tweet messages. In this way, we are laboring over hash tags, emojis or other tokens (usually on the scale of dozens to a few hundreds) rather than directly over the text messages (millions, usually). **Here we made two assumptions:**
 1. the sentiment of specific hash tag could be determined easily and accurately, like "vote4XXXX"; 
 2. one has to assume that, for most of the time, hash tags with clear sentiment bias are used according to their biases; in other words, expression methods like satire, irony and exaggeration are (relatively) rarely used. 

However, if sticking to these assumptions directly, one would encounter two difficulties: 
 1. of those highly used hash tags, most are neutral, like "trump2016" or "hillary2016". These hash tags are not necessarily used to express support or dislike. Thus could not be used directly. 
 2. among hash tags that are clearly biased towards certain sentiments, like "vote4XXXX" or "XXXXislier", more of them express negative rather than positive sentiments. As a results, the corpus one could get in this way is heavily biased towards the negative sentiment.

To solve the two difficulties, I developed a method of "dynamically expanding the set of hash tags with sentiment scores". It is worth mentioning that, for my current dataset (Presidential Election 2016), even using the expanded set of marked hash tags to estimate sentiments expressed by tweet messages, the result is already quite accurate. As explained above, this might due to the fact that tweet messages are usually succinct, and that more of the information is centered around hash tags and keywords. 
**Summarizing the Dynamic Iteration Method:**
 1. I manually marked ~200 hash tags that carries a very clear sentiment and that are most frequently used. [Marked Tags](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/tree/master/Ultra_Phase3v1/Data). 
 2. I noticed that people tends to call multiple hash tags at once. Thus, it would be possible to build up a "networked" dictionary for each hash tag, recording: what other hash tags are used together, what is the number of usage of those "networked" hash tags.
 3. I calculate sentiment scores of any specific neutral hash tag (or augmenting sentiment scores of clearly biased hash tags) using **all** hash tags that are inside its "networked" dictionary, through iteration, on a day-to-day basis. 
 4. when calculating sentiment scores of hash tags, the maximum number of iteration, the early stop criteria as well as the learning rate are set up in such a way that: for clearly biased hash tag, its sentiment score would at most change 50% per day; for neutral hash tags, its sentiment score could change almost 100% per day, much more flexible than clearly biased ones.

### Estimating Sentiment of Tweets using only Hash Tags



### Training a LSTM using a Semi-Automatically generated corpus

The Long Short Term Memory (LSTM) method has been in itself a very powerful method for NLP related applications. There has been [many](http://www.wildml.com/2015/10/recurrent-neural-network-tutorial-part-4-implementing-a-grulstm-rnn-with-python-and-theano/) [posts](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) elaborating how it works as well as [why](http://karpathy.github.io/2015/05/21/rnn-effectiveness/) it is so powerful. Like most neural network methods, LSTM is a supervised machine learning method. **However, what we are trying to do here, is not a strictly speaking supervised method**, because we don't have a perfectly reliable training dataset. 











The picture between is the top 150 most frequently used words from the corpus, where there are ~ 60000 unique words. One could see ~20% of them are hash tags.

![alt tag](https://github.com/Nimburg/Ultra_Phase1v5_Phase2v2_Phase3v1/blob/master/Results_Demo/Dictionary_Freq.png)

A few notes: my corpus for LSTM has 86979/4578/22888 sentences for training/validation/testing. For a deep training (more than 3 epoch ~2.5% stable error for validationa and testing), the number of mismatches between LSTM prediction and prediction solely from tags is ~3000 sentences. For a shallow training with early stop (1 epoch >3% error for validationa and testing), the number of misatches is ~4000 sentences.


-----------------------------------------------------------------------------
## Structure of this Project

- Phase1 is for data cleaning, creating MySQL database and extract hash tags for hand-marking
- PHase2 Part2 is for scoring tweets using results from Phase3 Part3. Tokenize tweet messages and create dataset for LSTM training and predicting. 
- v1_LSTM_Main is the LSTM codes, both training and predicting
- Phase3 Part1 is for extracting daily most used hash tags
- Phase3 Part3 is for dynamically expand the set of sentiment marked hash tags, on a day-to-day basis. Also extract selected hash tags for visualization. 


-----------------------------------------------------------------------------
## Introduction to the DataSet of this Project:

In order to achieved such objective, one has to download and analyze huge amounts of social media data. The reason that one needs such a large, continuous data set is because: 1st, since we are trying to understand individual's shifting opinions, we naturally what to have a continuous data set so as not to "miss something"; 2nd, since the fact that individual usually takes some time to have his/her mind changed, and that most people don't spend all day on twitter, thus each individual's activity is rather sparse in time, thus one need to collect data for a reasonably long period of time.

In this case, I have so far collected (commented on Nov 15th, 2016) ~40 days' worth of data, among which 18 contineous days in July (July 13th to Aug 2nd, 2016), 7 contineous days in October (Oct 15th to 21st, 2016) and a contineous data set from Oct 31st onwards. On average, each day's worth of data is ~7GB with 3 million tweets. I am only collecting tweets with geo-information indicating its user located within the United States. 

It is worth noting that 3 million tweets per day is apparently too few for such a user base. My guess is that the amount of tweets I could collect is limited by my twitter API query speed. However, since tweets are downloaded randomly (those that are got/missed by my API), it won't have any negtive effects on my analysis. 

Furthermore, for most parts of the data analysis, additional filters are put on tweet messages. For example, the number of words within a tweet message needs to be higher than 10, which are not: 1st, mentioned twitter users' names, 2nd 'https' links. Such filters will also reduce the number of tweets that are analyzed in Phase2 and Phase3. 


-----------------------------------------------------------------------------
## Technical Details

This set of codes are written in Python 2.7, using libraries that includes: theano, pymysq, nltk and other routine libs like numpy and pandas.

Phase1v5, Phase2v2 and Phase3v1 all demands MySQL as the way of managing huge data sets. 

The tokenization is done with the nltk package. 

The LSTM training and predictions codes are adapted from the tutorial codes [here](http://deeplearning.net/tutorial/contents.html). I adapted the training codes so that it would work on my data sets; I added prediction codes so that it would work with other parts of my Phase2v2. 

