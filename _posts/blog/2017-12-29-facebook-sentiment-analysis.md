---
layout: post
title: "Facebook Sentiment Analysis with NLTK"
modified:
categories: blog
share: true
excerpt: "What do Stirling residents feel about the waste services that Stirling Council provides? I had a go at finding out at the Stirling Public Data Hack recently."
tags: [coding, hackathon, nltk, open data, python]
image:
  feature: feature_image14.jpg
---

# Introduction - Stirling Public Data Hack


At the end of November I took part in the Stirling Public Data Hack, organised by [Wittin](https://www.wittin.co.uk). The purpose of the event was to explore ways in which [Stirling Council](https://my.stirling.gov.uk)'s Waste Services department could improve the services they provide to council residents by opening up some of their data and making it available to citizens who, like me, thought it might be interesting and useful to hack it to try and improve things.

After an initial orientation session and meeting with representatives from Stirling Council, I spent some time trying to get my head around the data sets. Trying to understand the contents of individual data sets, let alone how the pieces fitted together or how the data could be used to find insights/solutions, was 'challenging'.

(With hindsight, I wish I'd used [pandas](https://pandas.pydata.org/) and [jupyter notebooks](http://jupyter.org/) to do the exploratory data analysis. This would have really helped me get a handle on the data. However I didn't know pandas at that point, and wasn't particularly any more knowledgeable about jupyter.)   

When I came across the [Stirling Council facebook page](https://www.facebook.com/stirlingcouncil/) dataset, I wondered what I could with it. A question that I thought it would be interesting to explore was:

_'What do residents feel about the waste services that Stirling Council provides?'_

To help answer this I decided to undertake some sentiment analysis on the posts made by residents when commenting about waste-related matters on the council's facebook page.

I choose to do this because it:

* could provide the council with a genuinely useful insight into residents' views.
* would let me get down and dirty with some real-world data 
* would be a chance to develop my python skills
* would provide an opportunity to do some data visualisation
* would give me a chance to learn about something completely new i.e. sentiment analysis


In this post, I'll share the method I adopted on this task, and at the end, tell you how I could have done much more easily!

# Background

Sentiment analysis is the process of categorising opinions expressed in a piece of text to determine whether the author is expressing a positive, neutral or negative opinion.

# Method

I wrote a python script which:

1. loads the facebook comments from a CSV files, removes duplicates, cleans invalid values, and selects relevant comments.
2. uses the [NLTK (Natural Language Toolkit)](www.nltk.org) vader libraries to perform sentiment analysis
3. generates a bar chart for each year, showing sentiment analysis scores for that year. Each bar chart is saved as an image file.
4. generates a csv file containing aggregated sentiment analysis scores across all years

## Identifying Posts Relating to Waste 

1. A list of key words related to waste services is used to identify user messages which may be related to comments about waste services. This list is 'bin', 'waste', 'rubbish', 'refuse', and ‘collection’.
2. Any facebook posts that have been made by Stirling Council are not included in the analysis. 
3. Posts of less than ten words are not included in the analysis. (They are potentially too short to analyse accurately).
4. If the word ‘rubbish’ is used in a post as anything other than a noun, the post is not included. e.g. in ‘I went to see ***** at MacRoberts and she was rubbish’ rubbish is used as an adjective, so wouldn't be included. 

## Generating Sentiment Analysis Scores

1. All of the posts which have not be discarded should now relate to waste.
2. Each post is analysed using NLTK and give a positive, neutral and negative score. The neutral scores are not used.

## Aggregating Scores

1. Scores are grouped by month, and averaged by the number of posts this month with either a positive or negative score. For example, if there 20 posts in February 2016 with a combined positive score of 120, the average will be 120/20 i.e. 6.

# Output

1. A file sentiment_data.csv is generated which contains the dates of posts (grouped by month) and, for each month, the aggregated average sentiment score for this month, for positive and negative scores.
2. For each year from 2011 to 2017, an image file containing a bar chart is generated. The bar charts produced indicate the positive/negative opinions the facebook users express when commenting about waste on [Stirling Council’s facebook page] (https://www.facebook.com/stirlingcouncil/). Each bar chart covers one year and is broken down by month. For each month, two columns are shown: positive and negative. 

	Here's an example of two charts.

![]({{ "/images/facebook_sentiments11.png" | absolute_url }})

![]({{ "/images/facebook_sentiments12.png" | absolute_url }})


# Limitations

1. Some posts which contain non-ascii characters and which are unable to be converted into usable text are currently discarded.
2. There are some posts where NLTK (used for tagging words depending on the part of speech they represent) does not identify the word ‘rubbish’ as a noun correctly.  
3. There may be other key words relating to waste services that could be added.
4. We described earlier how the part of speech for the key word ‘rubbish’ was used to determine the particular meaning of this word in the context in which it would used. The same could be done for the other key words.

# What I Learned

Here's my takeaways from the experience:


* Using jupyter notebooks to interactively explore the data would have reduced the time needed for the task considerably
* using pandas for data cleaning, exploration and data selection would have done likewise
* using NLTK was interesting, but I'd need to spend a lot more time (ideally not in the middle of the night!) verifying the sentiment analysis undertaken with vader actually produced valid and meaningful results 
* I can see the power in matplotlib for data visualisation, but I need to spend more time working with it - I didn't find it particularly intuative
* I'm really not great at 4 a.m., no matter how much coffee I drink.