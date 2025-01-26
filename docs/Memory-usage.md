A few notes about memory usage

# Introduction

This page provides essential information on how memory is utilized by BIMserver and offers guidance on optimizing memory settings.

# Setup

When you got a warning or error that your server is 'out of memory' you probably don't have enough HEAP size allocated. Increase your HEAP size (in console, in tomcat, in jetty, or whatever you use). 
The necessary height of the HEAP is difficult to predict because it depends on the number of objects in your IFC model (and not on the size of the IFC file). We usually test with a 12Gb HEAP size server setup, but 220Gb servers are also known to be used by some people. 

# BerkeleyDB Cache

BerkeleyDB (the database engine the BIMserver uses) has a setting for the amount of heap memory it can use for caching (which will speedup reads), we have set it to 30%. So if you give your BIMserver 4GB of heap, it will soon be using more than 1GB of memory for caching. You can change this and other parameters only in the development setup or if you build [your own version of BIMserver](https://github.com/opensourceBIM/BIMserver) for production.

See [the BerkeleyDB documentation](http://docs.oracle.com/cd/E17277_02/html/java/com/sleepycat/je/EnvironmentMutableConfig.html#setCachePercent-int-) for more information.

# Compressed Oops

Running a 64bit system with less than 32GB of memory, you can use [Compressed Oops](https://docs.oracle.com/en/java/javase/21/vm/java-hotspot-virtual-machine-performance-enhancements.html#GUID-932AD393-1C8C-4E50-8074-F81AD6FB2444) (Ordinary Object Pointers). This feature reduces memory overhead, allowing for more efficient memory usage.

> Update: This is by default enabled on recent OpenJDK 6 and 7 implementations and onwards, so you probably won't have to do anything.
