---
layout: post
title: "Learning Using Open Data"
modified:
categories: blog
share: true
excerpt: "I've been rolling up my sleeves and getting stuck into learning to use the document database MongoDB by using an Open Glasgow data set. So what have I learned in the process?"
tags: [python, JavaScript, MongoDB, open data]
image:
  feature: feature_image_mongo.jpg
---

I've been rolling up my sleeves and getting stuck into learning to use the document database [MongoDB](https://www.mongodb.com/). I find that I learn best by doing, and that I can only really say I've understood something by applying it, so getting hands on is the best way for me to begin to understand this particular kind of 'noSQL' database.

## Open data

I've also been really keen to start working with real data (something I've not done before) so I downloaded a data set about absences in Glasgow primary schools from the [Glasgow Open Data portal](https://data.glasgow.gov.uk/) and gave myself a fairly simple question to try and answer: 

>_Which Glasgow City primary schools had the highest number of absences (as a percentage of the total possible attendances) in 2004-05?_
 

To help me understand the data set, I downloaded the data dictionary and used it to try and formulate how I'd go about answering the question.

## Where do I start?

At this point I realised I hadn't a clue how to use the tools I had and wanted to learn (i.e. MongoDB) to answer my question.

My next steps were to: 

* install and run MongoDB server and client software 
* practice importing and exporting to and from mongoDB from other data formats (JSON, CSV)
* understand how JSON objects are formatted and accessed
* try out basic CRUD methods against MongoDB
* try running a variety of queries against MongoDB (sorting, aggregating, filtering etc)
* Use MongoDB cursors to iterate through the results of a query

## Implement a simple solution

By this stage I had a fairly rudimentary understanding of (simple-ish) MongoDB queries, so started to develop a little JavaScript programme in [Studio 3T](https://studio3t.com/) to perform some simple calculations on the query results.

This gave me a first attempt at an 'answer'. 

## Python over JavaScript

But I started thinking that I didn't want to invest more time into learning JavaScript when Python and R are the most highly-used programming languages in data science, so I installed [PyMongo](https://api.mongodb.com/python/current/) and re-implemented my solution in Python.

## My results

I don't have a great deal of confidence in my current knowledge of data analysis or stats so I'll hold back on naming the school which had the highest percentage of absences. Let's call it 'Springfield Primary'.

What I found out was that 5.94% of pupil half-days<sup>1</sup> in Springfield Primary were .

At first I questioned this figure. It was almost 1% greater than the second highest-ranking school. But how different was it from the mean? 

## Questioning my results

Using [NumPy](http://www.numpy.org/) I calculated the mean number of absences as 0.76% pupil half-days. This made me question the data (or the method I'd used in my calculations, which I was beginning to doubt). I went back to the data set. Two 'facts' stood out. Firstly, Springfield Primary had 444 half-days of temporary exclusion of pupils (the second highest in the council). Secondly, the school had 5019 half-days of truancy, the highest in any Glasgow City primary school that year. 

This seemed like a big number. Was it an error in the data? On the assumption of the number of days in the school session being 190<sup>2</sup> this means that, on average, over 13 pupils per day were truant. That's a lot, but it _is_ feasible.

## Summing up

As a first attempt to use real data to answer a question, I'm really pleased. I learned some of the fundamentals of an unfamiliar database, how to use the NumPy python libraries, a bit about JSON, and, perhaps most importantly, how to use data to answer a question. 

My intrepretation of the data and my results left a lot to be desired but hopefully that will get a lot better after we take our statistics model.




<sup>1</sup>half-days on which pupils participate in the programme of educational acvities, arranged by the school, within or outside of the school environment.

<sup>2</sup>based on current information - no historical information about 204-2005 to support this.



