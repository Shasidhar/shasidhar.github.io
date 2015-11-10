---
layout: post
title:  "Apache Spark Performance Tuning"
date:  2015-11-02 20:32:27
categories: Spark
---

As Apache Spark is the popular framework now a days for Big Data solutions,in this blog we will try to analyse different key factors that can be used to improve the performace of Spark application in production. Spark applications can be improved if we look into the following factors

## Quantity of the data shuffled.

As you know, shuffle is the most costliest operation in Spark which involves movement of the data in the cluster. You should always try to decrease size of data for shuffling to increase application performance. One common solution to achieve this is, always *filter* data before shuffle phase rather than after. 

## Degree of Parallelism

* Always try to avoid the empty partitions. Generally if we have a filtering operation the result of the filtering operation reduces the size of data. If the filter operation is explicitly selective then there is always a chance that filtering operation ends up giving some of the partitions with very less data or some times no data. As soon as the size of data per partition decreases the sitaution arises where, time to launch the tasks for the empty partitions becomes overhead and reduces the performance significantly.Solution here is to use **coalesce** opeartion and reduce number of partitions after filtering.

* If your job is taking a long time and your cluster is not utilised completely, try to use **repartition** operation. Repartitioning allows you to increase/decrease the number of partitions of your job. In this case try to increase the number of partitions so the the cluster is used efficiently. It can be little bit of expensive operation because when you call repartition some data is shuffled across the cluster. But if the operation you are doing is expensive and if you can use your empty cluster cores to paralellise the process , it can be beneficiary to call **repartition** on your data.

## Choice of serializer

In spark serialization happens whenever we perform **cache** and **shuffle** operations. As i have already mentioned shuffle is the most costliest operation in Spark. By default Spark uses java serialization which is very robust and has support for all types of data but it is very slow. Apart from this, spark has built in support for **Kryo** serialization.
**Kryo** serialization is faster and has out of the box support inside Spark. To use it follow the below steps

* Change the spark configuration settings to use **Kryo** serializer.

{% highlight scala %}
val conf = new SparkConf()
conf.set("spark.serializer","org.apache.spark.serializer.KryoSerializer")
{% endhighlight %}

* Register all the classes which are serialized. Registration helps kryo to reduce the amount of metadata written as part of the serialization and decreases unwanted overhead. This adds up some of the boiler plate code inside your application.
{% highlight scala %}
conf.set("spark.kryo.registrattionRequired","true")
conf.registerKryoClasses(Array(classOf[*yourClass*],classOf[*yourClass1*]))
{% endhighlight %}

## Cache Format

In Spark whenever we cache the data it is stored using **MEMORY_ONLY** level in form of deserialised JVM objects. This is very fast in the performance perspective because it can be retrieved directly without any cost. It has some side effects, one of them being increase in the GC pressure as there is lot of data sitting inside your cache.

To reduce the side effects of *cache* spark provides two more levels of caching.

1. MEMORY_ONLY_SER - This mode decreases GC problems by saving all of the objects in serialized format as one object from the perspective of java memory management. The only trade off here is, since data is stored in serialized format spark does deserialization on the fly to access data. This can be little bit time consuming than the other one.

2. MEMORY_AND_DISK - It avoids recomputation of the cached data when ever we access the data from disk. The only trade off here is , since we are hitting disk for the data it will be slow.

## Other aspects

* Even though spark is an horizontally scalable engine, it is good to have more number of executors with optimal heap size rather than having executors with more memory. The optimum size of an executor is 64GB.

* Use LZF compression to improve shuffle performace.
{% highlight scala %}
conf.set("spark.io.compression.codec","lzf")
{% endhighlight %}

* Turn on speculative execution to help prevent stragglers.
{% highlight scala %}
conf.set("spark.speculation","true")
{% endhighlight %}

* Make sure spark has enough disks to store the shuffled data. Normally spark tries to move data across disks if memory runs out. It is set using following properties

1. In YARN mode, inherits YARN's local directories

2. SPARK_LOCAL_DIRS in mesos/standalone mode

Note: All the above mentioned tweaks may not yield performance improvement in every spark application. The goal of putting all these paramaters in one place is to provide overview for users what are the places one can try to improve the performance of spark application.

Hope you enjoyed reading this blog and it benefits you.