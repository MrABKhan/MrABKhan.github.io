--- 
layout: post    
title: "Using GrayLog for Distributed Log Management"  
excerpt_separator: <!--more-->
date: 2018-12-13 16:20:00
tags: [graylog,graylog,enterpriselogging,linux,windows]
bakar:
   name: AbuBakarKhan
   email: theawesomekhan@gmail.com
   web: http://www.theautodidact.me
author: bakar
keywords:
---    


# What is GrayLog!? 
Graylog is a powerful platform that allows for easy log management of both structured and unstructured data along with debugging applications. It is based on Elasticsearch, MongoDB, and Scala. Graylog has a main server, which  receives data from its clients installed on different servers, and a web interface, which visualizes the data and allows to work with logs aggregated by the main server. Thus, when integrated properly with a web app, Graylog helps engineers to analyze the system behavior on almost per code line basis.

## Our Usage of GrayLog
During my internship our primary usage was for accumulating the logs from our CI/CD server logs to a single server and GrayLog was perfect for this. This allowed the Dev Ops team to view the logs in real time without having to read the logs manually for each server. Along with helping in debugging the various builds, it also allowed them to query the "database" of logs for finding useful statistics regarding the different components of the application, but that was out of my scope as I was responsible for deploying an instance of Graylog and then configuring it's collectors on Linux/Windows to send logs to the central GrayLog Server.

## How GrayLog Works (Simplified):
I won't go onto the details of how GrayLog works internally since that information can be found in [GrayLog's Documentation](http://docs.graylog.org/en/2.5/). Thus the following image(taken from the documentation) shows a simplified way of how GrayLog works with it's collectors.
({{ site.baseurl }}/images/graylog_arc.png)


