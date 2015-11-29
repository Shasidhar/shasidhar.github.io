---
layout: post
title:  "Mapreduce History"
date:  2015-01-30 20:32:27
categories: jekyll update
---

####Map Reduce are the operators from LISP

Map-reduce are the two different operators in LISP. Lisp is a functional language where everything is expressed as a function. But in hadoop map-reduce they are not the same implementation, only the ideas are inspired from lisp. The similarities between Lisp MR and hadoop MR is,map operates on the input data, reduce operates on map output data, reduce cannot start until map finishes. The way in which both of these functions operate makes them suitable for parallel programming. Since we code everything as mapreduce in hadoop framework , Mapreduce is the programming paradigm. 

Lisp convention of expressing map and reduce operations

1. Map -> *(map ‘length ‘(() (a) (ab) (abc)))  -> (0,1,2,3)*

2. Reduce -> *(reduce '+ '(0 1 2 3)) -> 6*

####MapReduce is a divide and conquer strategy

In any map reduce programs there will be two procedures, map() and reduce(). Map is the operator which process data at granular level where as reduce process grouped data. We can call map phase as dividing phase which gives the ability for reducer to conquer data. This ability of map() and reduce() procedure is the key which makes map-reduce well suited for parallel programming. 

####Adopted by Google to solve distributed system processing 

If we think of google web search it involves mainly following steps:

* Crawl Web (Spiders) -> Sort pages by URL (Sorting in MR)-> Remove Junk -> Rank URLs (Url, Ranks) -> Create Inverted Index (Word,Urls)

* Reverse weblink graph is a pair of (target,source(urls)) pair.

* Ranking based on access frequency (Word count) + reverse weblink graph.

* Inverted index (Word, list[Urls]) 

If we look into each stage mentioned above we can achive each of them with individual map-reduce program. Map reduce is first adapted in Google by Jeffrey and Sanjay at 2004.Interestingly these are some the first map reduce jobs implemented in goolge. 

As we know google published their ideas of map reduce in a IEEE paper, and some of the important learnings are as follows :

1. Restricting the programming model makes it easy to parallelize and distribute computations, and to make such computations fault-tolerant.
2. Network bandwidth is the most important resource, it is the main reason to write the intermediate results into local disk and run the combiners which can significantly save the network bandwidth.
3. Redundant execution can be used to reduce the impact of slow machines.

####MapReduce is generic idea, hadoop is one of the implementation

Its obvious now , as there are already different places where we can find these map-reduce operations as mentioned above. So now you may be thinking what is hadoop doing with map reduce. The key point we need to identify is, Hadoop is one of the implementation of the Map Reduce idea. Its implementation is suited for distributed computing with aim of removing the worry of distribution and paralellism from the programmers. 

*Hope you got the idea of what this mapreduce is all about, to summarize it is a generic algorithm which hadoop implements in one way which is suitable for handling big data*