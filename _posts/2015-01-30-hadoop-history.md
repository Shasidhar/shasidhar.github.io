---
layout: post
title:  "Hadoop history"
date:  2015-01-29 20:32:27
categories: jekyll update
---


Apache Hadoop is an open source software project that enables distributed processing of large data sets across clusters of commodity servers. It is designed to scale up from a single server to thousands of machines, with a very high degree of fault tolerance.

Let’s briefly know how Hadoop evolved.

Hadoop was created by Doug cutting, the creator of Apache Lucene , which is the widely used text searching library. 

Doug cutting was ambitious about building a web search engine from scratch because of two aspects, 

1. Its a very complex engine with moving parts which requires an operational team.
2. It involves a very difficult software design.

Hence, he started to write his own web search engine along with his colleague Mike ,they named it as **Nutch**. However, Doug cutting was sure that his project cannot scale to millions of pages in web. That is when in 2003 a revolutionary paper came out from Google regarding their distributed file system called **Google File System** , which was used in their production.

The Google file system was very efficient in solving time consuming administrative tasks like storage nodes management etc, they realised their project Nutch also requires this kind of file system. Hence, in 2004 they set out to implement the same idea for open source, called Nutch Distributed System.

By the time they had their first working version, Google introduced the idea of **Map Reduce** to the world i.e., in 2004.That seemed to be a very good idea for Doug, within 2005 every task in Nutch was using **Map Reduce**.

Both **NDFS** and **Map Reduce** were beyond the realm of search. Further, in 2006 they moved out of Nutch and started to create an independent sub-project under Lucene called as **Hadoop**. At the same time Doug cutting went to work with Yahoo!, which provided a dedicated team and the resources to turn Hadoop into a system that ran web at scale .This was demonstrated in 2008 and Yahoo announced to the world that, its search engine works on 10000 node Hadoop cluster.

In 2008 Hadoop became an independent project under Apache license confirming its own success and strong developer community. By this time, Hadoop was used by many of the companies like Facebook, New York Times etc. Hadoop broke the world record by sorting a terabyte of data, running 900 node clusters to sort 1 terabyte of data in 200 seconds. Later a team at Yahoo! used Map Reduce to sort the same amount of data , it was only in 62 seconds. 

These kinds of results and bench markings show the incredible underlying capabilities of the Hadoop.