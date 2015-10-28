---
layout: post
title:  "Zookeeper - Dynamic Configuration Source with Archaius"
date:  2015-10-26 20:32:27
categories: jekyll update
---


Dynamic configuration is the ability to change the configuration while system is running. Archaius is the configuration management library which can retrieve properties from several sources and load them into the system dynamically.For a distributed system Apache Zookeeper is the default coordination service. This blog shows 

* How can we create custom listeners for the changes in zookeeper configuration using Zookeeper Watches.

To accomplish the above task we need to have to components

1. ZookeeperConfigSource
2. ZookeeperUpdateListener

Project code can be accessed in [GitHub](https://github.com/Shasidhar/zookeeper_dynamic_config_source_with_archaius)

###ZookeeperConfigSource

Here we are going to use apache curator framework for zookeeper related operations. We need to specify parent node,like `/<app>/config` as the root node which acts as a parent for all configurations. As you know the data in zookeeper is stored in `ZNodes` and each `ZNode` is a node in Zookeeper's tree similar to a filesystem.

This component initiates listeners on the node specified above with the curator client. By doing so, we can get an update in the listeners whenever data changes under `/<app>/config`. To accomplish this we use `PathChildrenCache` recipe from curator framework. This recipe allows us to have listeners and customize them.

Steps:

* Create a Zookeeper Config Source of type `WatchedConfigurationSource` provided by Archaius.
* Override 3 methods `getCurrentData`,`addUpdateListener`,`addUpdateListener`
* Define a method to catch different actions and trigger an event

Refer [ZookeeperConfigSource](https://github.com/Shasidhar/zookeeper_dynamic_config_source_with_archaius/blob/master/src/main/scala/com/shashidhar/zookeeper/ZookeeperConfigSource.scala) for code.

###ZookeeperUpdateListener
We need to extend `WatchedUpdateListener` interface provided by archaius library and override `updateConfiguration` method.When ever a change occurs in the source on which this listener is configured the `updateConfiguration` event is trigerred.

Steps:

* Create a listener for ZookeeperConfigSource
* Start the listeners for ZookeeperConfigSource
* Listen for updates in `updateConfiguration`. 
* Perform requried action in `updateConfiguration` method.


Refer [ZookeeperUpdateListener](https://github.com/Shasidhar/zookeeper_dynamic_config_source_with_archaius/blob/master/src/main/scala/com/shashidhar/zookeeper/ZookeeperUpdateListener.scala) for code.

Finally we have to bind the Source and Listener. 

{% highlight scala %}

object ZookeeperMain {
  def main(args: Array[String]) {
    //root node on which we want to listen
    val zkConnectionString = "/<app>/config"
    //Create curator client for the zookeeper
    val client = CuratorFrameworkFactory.newClient(zkConnectionString,
    new ExponentialBackoffRetry(1000, 3))
    //Create zookeeper source
    val source = new ZookeeperConfigSource(client,zkConnectionString)
    //Create listener object
    val listener = new ZookeeperUpdateListener(source)
    listener.init()
  }
}
{% endhighlight %}