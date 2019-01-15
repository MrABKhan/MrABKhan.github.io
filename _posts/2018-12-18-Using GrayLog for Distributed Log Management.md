--- 
layout: post    
title: "GrayLog: Installing GrayLog on Docker"  
excerpt_separator: <!--more-->
date: 2018-12-13 16:20:00
tags: [graylog,graylog,enterpriselogging,linux,windows]
image: 
bakar:
   name: AbuBakarKhan
   email: theawesomekhan@gmail.com
   web: http://www.theautodidact.me
author: bakar
keywords:
  - graylog
  - logging
  - how to use graylog
  - graylog configuration
  - developer journal
  - graylog usage
  - graylog docker
  - docker
  - distributed logging
  - enterprise logging
  - theautodidact.me
---    


## What is GrayLog!? 
Graylog is a powerful platform that allows for easy log management of both structured and unstructured data along with debugging applications. It is based on Elasticsearch, MongoDB, and Scala. Graylog has a main server, which receives data from its clients installed on different servers, and a web interface, which visualizes the data and allows to work with logs aggregated by the main server. 
<!--more--> 
Thus, when integrated properly with a web app, Graylog helps engineers to analyze the system behavior on almost per code line basis.

## Our Usage of GrayLog
During my internship our primary usage was for accumulating the logs from our CI/CD server logs to a single server and GrayLog was perfect for this. This allowed the Dev Ops team to view the logs in real time without having to read the logs manually for each server. Along with helping in debugging the various builds, it also allowed them to query the "database" of logs for finding useful statistics regarding the different components of the application, but that was out of my scope as I was responsible for deploying an instance of Graylog and then configuring it's collectors on Linux/Windows to send logs to the central GrayLog Server.

## How GrayLog Works (Simplified):
I won't go onto the details of how GrayLog works internally since that information can be found in [GrayLog's Documentation](http://docs.graylog.org/en/2.5/). 

![GrayLog Simplified]({{ site.baseurl }}/images/graylog_arc.png)

### Sidecar/Collector System of GrayLog:
GrayLog implements a system with different "collector" programs running to collect logs and report them to the central server or server cluster.

There are then two versions of these available : 
1. GrayLog Collector 
2. GrayLog Collector Sidecar

#### GrayLog Collector
GrayLog Collectors are lightweight java applications which read log files natively and send them to the server( or server cluster ) using the [GELF](http://docs.graylog.org/en/2.5/pages/gelf.html) format.
#### GrayLog Collector Sidecar
These are the newer and more beefed up version of the Collector. GrayLog Collector Sidecars are complete configuration managment systems which work with different log collection systems as in WinLogBeat,NxLog etc. They are connected via a REST API which enables the GrayLog Server to change configurations or restart/run/shutdown sidecars without needing to manually handle them on each host machine.


## How to Use GrayLog with Windows/Linux Logs using Collectors :
Thus in this post, I am primarily going to explain how to use GrayLog Collectors with a GrayLog Server on a Linux or Windows Based System. 

### 1. Installing GrayLog Server:
GrayLog can be installed on a number of [Operating Systems](http://docs.graylog.org/en/2.5/pages/installation.html) and [Virtual Machines](http://docs.graylog.org/en/2.5/pages/installation/virtual_machine_appliances.html). I'm going to use Docker for this tutorial.
#### Requirements:
1. [Graylog for Docker](https://hub.docker.com/r/graylog/graylog/)
2. [MongoDB](https://hub.docker.com/_/mongo/)
3. [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/docker.html)

#### QuickStart GrayLog on Docker
To simply start GrayLog without any other customization, we can run the following commands to create the environment for GrayLog to run.
##### 1. Run MongoDB
`
$ docker run --name mongo -d mongo:3
`
##### 2. Time for ElasticSearch
`
$ docker run --name elasticsearch \
    -e "http.host=0.0.0.0" -e "xpack.security.enabled=false" \
    -d docker.elastic.co/elasticsearch/elasticsearch:6.5.1
`
##### 3. Linked and Loaded for GrayLog
`
$ docker run --link mongo --link elasticsearch \
    -p 9000:9000 -p 12201:12201 -p 514:514 \
    -e GRAYLOG_WEB_ENDPOINT_URI="http://127.0.0.1:9000/api" \
    -d graylog/graylog:2.5
`

Note that we have to use the same ports opened here through *-p 9000:9000* for our inputs in TCP or if you want to open UDP ports then *-p 9000:9000/udp*.

### Next Steps
GrayLog should be up and running after this setup on your docker! I will update setting up collectors on windows and linux in the next part of this blog :)

**-  Bakar**