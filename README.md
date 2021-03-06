StormIt
=======
**StormIt** is a Clojure DSL for Apache Storm which allows programming Apache Storm topologies using StreamIt like constructs. [StreamIt](http://groups.csail.mit.edu/cag/streamit/) is a programming language and compiler infrastructre for developing large streaming applications. [Apache Storm](http://storm.incubator.apache.org) is a distributed realtime computation system. 

Concepts
========

All the stream processing construct in **StormIt** was borrowed from StreamIt stream processing language. Even though these two languages served different purposes and work on different environment (Distributed vs Multi-core), concepts are same and only the underline implementation is different. Currently there are three main constructs in **StormIt**. I am planning to improve this to include more constructs as I learn more about stream processing languages.

* **Filter** - Filters are the main actors which operates on data streams. Even though [Apache Storm](http://storm.incubator.apache.org) differentiate input sources and stream operators, StormIt uses same filter construct for both. StormIt differentiate sources from stream operators based on its type. StormIt filter type defines fields in tuples it consumes and fields in tuples it produces. 
* **Pipeline** - Pipeline in **StormIt** is same as pipeline construct in [StreamIt](http://groups.csail.mit.edu/cag/streamit/). Its a sequence of stream operators which process incoming stream one after the other.
* **Split-Join** (Under development)
* 

Sample
======

Simple input source.

```clojure

(sfilter int-source [] [[] -> ["int"]]
         (init [max 1000])
         (work {:push 1}
               (let [i (rand-int max)]
                 (Thread/sleep 100)
                 (spush [i]))))
```

Simple stream operator.

```clojure
(sfilter incr-and-print [] [["int"] -> []]
         (init [increment 2])
         (work {:pop 1 :push 0 :peek 0}
               (let [i (.getInteger (spop) 0)]
                 (info (+ i increment)))))

```

Sample stream processing pipeline in StormIt.

```clojure
(spipeline simple-app []
           (add int-source)
           (add incr-and-print))
```
News
====

* 04/24/2014 10PM - Simple pipelines are working now.
* 

TODO
====
* Implement **Split-Join** support in StormIt.
* Implement 'Word Count' sample in **StormIt**.
* Implement 'FIR Filter' in **StormIt**.
