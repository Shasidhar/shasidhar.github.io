---
layout: post
title:  "Mapreduce History"
date:  2015-01-30 20:32:27
categories: jekyll update
---

####Map Reduce are the operators from LISP

Map-reduce are the two different operators in LISP. Lisp is a functional language where everything is expressed as a function. But in hadoop map-reduce they are not the same implementation, only the ideas are inspired from lisp. The similarities between Lisp MR and hadoop MR is,map operates on the input data, reduce operates on map output data, reduce cannot start until map finishes. The way in which both of these functions operate makes them suitable for parallel programming. Since we code everything as mapreduce in hadoop framework , Mapreduce is the programming paradigm. 

(map ‘length ‘(() (a) (ab) (abc)))  -> (0,1,2,3)

(reduce '+ '(0 1 2 3)) -> 6

####MapReduce is a divide and conquer strategy

In any map reduce programs there will be two procedures, map() and reduce(). Map normally is a operator which does the action on each piece of data where as reduce operates on grouped data. We can call map phase as dividing phase which gives the ability for reducer to conquer data and by the time reduce stats we will be in a position to conquer the data. The meaning here is simple, in map phase we treat the data independently and transform the data into the format which we later use in reduce phase in a better position. This ability of map() procedure is the key which makes map-reduce well suited for parallel programming. 

####Adopted by Google to solve distributed system processing 

Crawl Web (Spiders) -> Sort pages by URL (Sorting in MR)-> Remove Junk -> Rank URLs (Url, Ranks) -> Create Inverted Index (Word,Urls)

Reverse weblink graph is a pair of (target,source(urls)) pair.

Ranking based on access frequency (Word count) + reverse weblink graph.

Inverted index (Word, list[Urls]) 

Map reduce is first adapted in Google by Jeffrey and Sanjay at 2004. Their main motivation lies in the idea of these two functions. If we look into map operation we realize that each application of the function to a value can be performed in parallel since there is no dependency of one another. The reduce function can only take place once the map is finished.Some of the interesting problems which are part of the google's web search engine which can be easily expressed in MR way are 

1. Count of URL access frequency
2. Reverse Web link graph
3. Inverted index

Interestingly these are some the first map reduce jobs implemented in goolge. Some of the learnings which google mentioned in their map reduce paper is: 

1. Restricting the programming model makes it easy to parallelize and distribute computations, and to make such computations fault-tolerant.
2. Network bandwidth is the most important resource, it is the main reason to write the intermediate results into local disk and run the combiners which can significantly save the network bandwidth.
3. Redundant execution can be used to reduce the impact of slow machines.

####MapReduce is generic idea, hadoop is one of the implementation

You can see map and reduce as functions or operators or procedures etc in different languages and frameworks. You can say hadoop is one of the implementation among them. Hadoop has implemented them in the way it is suited for distributed computing and removing the overhead of parallelism and storage systems from the programmers control. Hadoop framework is very good in handliing the data, assigning the tasks based on the data locality, running the redundant executions whenever necessary without himdering the system throughput etc.