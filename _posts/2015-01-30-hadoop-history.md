---
layout: post
title:  "Hadoop history"
date:  2015-01-29 20:32:27
categories: jekyll update
---
Hadoop was created by Doug cutting, the creator of apache Lucene , widely used text searching library. Doug cutting was ambitious about building a web search engine from scratch because of two aspects, its a very complex engine with moving parts which requires an operational team and it involves a very difficult software design. He started to write his own web search engine which is called as Nutch along with his colleague Mike. 

It was the time when Doug cutting was sure that his project cannot scale to millions of pages in web, the revolutionary paper came out from google regarding their distributed file system called Google File System in 2003. This was the file system which Google uses in its production. Soon they realised this is the kind of system Nutch requires for large scale web search engine as it would solve the time being spent on administrative tasks like storage nodes management etc. In 2004 they set out to implement the same idea for open source, the Nutch Distributed File System.

In 2004 google introduced mapreduce idea to the world. Within 2005 every task in Nutch was using MapReduce. Both NDFS and MapReduce was beyond the realm of search. In 2006 they moved out of nutch and started to create an independent sub-project under Lucene called as Hadoop. At the same time Doug cutting joined Yahoo! which provided a dedicated team and the resources to turn hadoop into a system that ran web at scale and this was demonstrated in 2008. Yahoo announced to the world , its search engine works on 10000 node hadoop cluster.

In 2008 Hadoop became a independent project under apache license confirming its own success and strong developer community. By this time, Hadoop was used by many of the companies like Facebook, New York Times etc. Hadoop broke the world record to sort a terabyte of data. Running 900 node cluster to sort 1 teratebyte of data in 200 seconds. Later a team at Yahoo! used MapReduce to sort the same amount of data , it was only in 62 seconds. These kind of results and bench marking shows the incredible underlying capabilities of the Hadoop.
