
# COS598F: Spark and c8 Cluster Intro

Powerpoint for this lecture: https://www.dropbox.com/s/4vh1sb5z0gi63k4/cos598f-spark-c8.pptx

## Spark

Spark is a distributed computing framework that allows one to easily write fault-tolerant distributed applications. Find more information about Spark here: http://spark.apache.org/docs/latest/quick-start.html.

Following the webpage, you can download Spark and run it on your local machine. Next we are going to show you how to run spark application on our cluster.

## c8 cluster

We have setup a Spark standalone cluster on our c8 cluster. Access its WebUI with http://mmx.cs.princeton.edu:8111/, there are 6 workers currently.

To submit jobs to the spark cluster, first you need to apply for an account for c8:
https://docs.google.com/forms/d/1RTYewndE6WfTQOrAwrN6wCYkWARjEynic8PDrNI6l8Y/viewform?usp=send_form

After obtaining username and password information, first log in to the bastion host: 
```
ssh yourusername@mmx.cs.princeton.edu
```

then ssh into one of the computing nodes. The nodes are from e0-e31, and f0-f31. Unfortunately many of them are down currently.
We use f1, f3, f4, f5, f6, f8 for our Spark cluster, so you can ssh into one of those machines to submit a job, e.g. `ssh f1`. The home directory is under `/memex/yourusername`, and shared across all the nodes. However we don't have much space left, so please don't overuse it. Put the following two lines in your `~/.bashrc` in order to run Spark:

```
export JAVA_HOME=/etc/alternatives/jre_1.7.0
export SPARK_HOME=/memex/linpengt/cos598f/spark-1.6.0-bin-hadoop2.6
```

The course files are under `/memex/linpengt/cos598f`. There is a spark distribution under `/memex/linpengt/cos598f/spark-1.6.0-bin-hadoop2.6`. `cd` into that directory, and execute:

```bash
bin/spark-submit  --class "SimpleApp"   --master spark://fat:7077  ../java_wordcount/target/simple-project-1.0.jar
```

And look for output line like
```
Lines with a: 58, lines with b: 26
```
Congratulations! You have run your first Spark application. Read this page to find all the information about submitting jobs to a spark cluster: http://spark.apache.org/docs/latest/submitting-applications.html

## Assignment

There is a PageRank example application for Spark here: https://github.com/apache/spark/blob/master/examples/src/main/java/org/apache/spark/examples/JavaPageRank.java

You are required to implement HITS, another link analysis  algorithm (https://en.wikipedia.org/wiki/HITS_algorithm) based on this example, and compare their results.

Steps:

1. Compile `JavaPageRank.java` into a Spark standalone application following [these instructions](http://spark.apache.org/docs/latest/submitting-applications.html). You might need to install `maven` locally for compilation.
2. Modify the program if necessary, and run it on the Google link data: https://snap.stanford.edu/data/web-Google.html
3. Implement HITS algorithm based on PageRank (you can put two algorithms in two class files under the same maven project, and compile them into the same jar). Run it on the Google data, and compare the results against PageRank.
4. Copy the jar file to your home folder via `scp`, and run them on the larger LiveJournal data in `/memex/linpengt/cos598f/soc-LiveJournal1.txt`. Vary the number of cores used by your application from 4 to 24, and record the running time.
