---
layout: post
title:  "Understanding Mapreduce idea"
date:  2015-01-30 20:32:27
categories: jekyll update
---

In this post we try to understand the mapreduce idea by writing simple java programs. We will see the consequences we face on each stage. To begin with let's start with a problem.

**Problem Statement** : **Given a collection of sales find frequency of each item**

* Solution 1 : Simple Java way

Take the input file.
Parse each line of sale and add each item to the HashMap<String,Long>. 
If the item alredy presents in map increase the count for the item in HashMap

{% highlight java %}

String filePath = args[0];
BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
String line;
Map<String,Long> countValues = new HashMap<String,Long>();
    while ((line=bufferedReader.readLine())!=null) {
        String columns[] = line.split(",");
        String item = columns[2];
        if(!countValues.containsKey(item)){
              countValues.put(item,1L);
        } else {
           long currentCount = countValues.get(item);
           countValues.put(item,currentCount+1);
         }
      }
System.out.println(countValues);
{% endhighlight %}


This approach is direct way, not mapreduce way and we cannot scale it for the large data. Next solution takes the mapreduce path.

* Solution 2 : Solution using M/R algorithm

*MapReduce Algorithm*

1.Take each line and split into columns.

2.For each word output => (word,1)

3.Group output by key

4.Total all the one's to get final sum

Refer [MapReduceMain](https://github.com/Shasidhar/mr_in_java/blob/master/src/main/java/com/shashidhar/mrinjava/MapReduceMain.java) for code.

This is in mapreduce way but it is not yet parallel in nature. Next step is to implement the same idea using java threading.

* Solution 3 : 


We take the same idea of solution 2 but we run two mappers now. Line number determines which mapper a line is sent for processing.All lines with linenumber less than 10 to mapper-1 and lines with linenumber more than 10 to mapper-2. 

We create a thread pool using java executer service and add our two mappers into this thread pool. Once we add our mappers we will shutdown the executor so that no other process is added to executor service. 

Next we will block the main program until all the mapper tasks are completed, since we know we should not start reduce until all map operations is done.

Once mappers are completed we will add all the map results, and perform the group operation.
After grouping we will perform the reduce operation.

Refer [MapReduceMainParallel](https://github.com/Shasidhar/mr_in_java/blob/master/src/main/java/com/shashidhar/mrinjava/MapReduceMainParallel.java) for code.


As we observe here we still get the same result even though we run mappers in two different threads. This is the main idea behind the map reduce in hadoop.

So lets not waste time and start looking into the mapreduce in hadoop.

**Map reduce in hadoop**

* Phase 1: Splitting
* Phase 2: Assign map tasks based on data locality
* Phase 3: Map output is sorted and stored in local file system of the node to avoid un necessary replication in hdfs
* Phase 4: Shuffling, as soon as one mapper writes output to local file reducer calls shuffling. Shuffling is nothing but copying the map output files to reduce instance
* Phase 5: Merging , is the process of merging the map output files into one file.
* Phase 6: Grouping, is the process of grouping the map output based on map output key. It generates the reducer input [mapOutputKey,List[Values]]
* Phase 7: Reduce , final phase where reducer logic written by the user is applied on the each group of the unique map output key.

*This blog illustrates how the idea of mapreduce can be achieved even without use of hadoop in Simple java. It lays out the different pieces of hadoop mapreduce implementation for better understanding*