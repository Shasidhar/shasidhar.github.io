---
layout: post
title:  "Secondary Sorting in Hadoop"
date:  2015-01-29 20:32:27
categories: jekyll update
---

I have read many blog’s about secondary sorting when I was learning hadoop. Most of these blogs tries to explain it conceptually which is not sufficient as it is kind of hack in hadoop. In this blog let’s see how that can be understood in a better way .We see how data changes internally in each step of secondary sort.


Let’s consider the problem statement given below and solve it with secondary sort.

**We have a sales data where there are three columns `customerId, itemId, amountPaid` with these three columns what we want to is the maximum amount paid by the customer for a single item**

####**Example data**


CustomerId    | itemId    | amountPaid
:-------------: | :-------------: | :------:
1  | 1  | 100
2  | 1  | 200
1  | 3  | 1000
1  | 2  | 400
1  | 5  | 350
2  | 6  | 800


For this problem the basic solution is to write map-reduce where mapper will emit key: customerId, value: amountPaid. We get all the amountPaid values for each customer in reduce side. We can easily sort these values by using any sorting technique in reducer.

But as I mentioned we want to solve it in a different way. Now, think of mapreduce frame work in hadoop, how it works. We all have heard about the word sorting in hadoop mapreduce framework. Yes, right!! All the map output keys are sorted by default in hadoop.

Secondary sort is an idea where we keep map output key such a way that we get only required keys in the reduce side, other keys are ignored. To do this we have to make our key value pair from  `key:customerId,value:amountPaid`  to  	`key:<customerId,amountPaid>, 	value:NullWritable`.

There are three main phases in mapreduce between mapper and reduce stages, and we have to manipulate all of them to have our solution.

1. *Sorting* : Sorting happens after mapper is finished with all input data. To have our composite key sorted in the required order, customerId by (ascending) and amountPaid by (descending) order, we have to overwrite sorting for compositeKey.

*Output* : `<1,1000>,<1,400>,<1,350>,<1,100>,<2,800>,<2,200>`

2. *Partitioning* : Partitioning determines the reducer for every map-output key. If we leave out composite key here each key can go to different reducer, but we want all the keys for a particular customer should go to the single reducer. To do that we have to overwrite partitioning method. Lets think we have two reducers in our mapreduce job then the output would be,

*Output* : `reducer1: <1,1000>,<1,400>,<1,350>,<1,100>`
		   `reducer2:  <2,800>,<2,200>`

3. *Grouping*: "Grouping" groups all the map-output key value pairs of the mapper to generate single key with group of value input for the reducer. In our output we still have 4 distinct keys for customerId 1 and 2 distinct keys for customerId 2. But we want only the key with maximum amount to be selected for reduce function ignoring all other keys. To do that we have to overwrite grouping. By default grouping uses complete key `<customerId,amountPaid>` for grouping, we change it to use only `customerId` from our key.

*Output* : `<1,1000>`
		   `<2,800>`
 
 If we look into result of grouping, we are getting only the composite key with maximum amountPaid for each customer. There we got solution for our problem!!!