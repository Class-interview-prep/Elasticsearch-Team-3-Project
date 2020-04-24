# Elastic Stack  Installation on CentOS 7
---





## Introduction
 ### What is ELK Stack
The Elastic Stack is a collection of open-source software produced by Elastic which allows you to search, analyze, and visualize logs generated from any source in any format, a practice known as centralized logging. 

![ELC STACK](https://sysadminwork.com/wp-content/uploads/2018/09/Elasticsearch-Logstash-Kibana-ELK-Stack-1.png)


The Elastic Stack has four main components:

1. [Elasticsearch](elasticsearch) - is used for storage, analysis, search by logs.
1. Logstash - service for collecting logs and sending them to Elasticsearch. In the simplest configuration, you can do without it and send logs directly to Elasticsearch. But with logstash it is more flexible to do it.
1. Kibana - is web panel for working with logs.
1. Beat - agents to send logs to Logstash. They are different. I will use Filebeat to send data from linux and Winlogbeat text logs to send logs from Windows logs.


This tutorial provides you how to install the Elastic Stack on a CentOS 7 server. At the end of this tutorial, you will have all of these components installed on a single server, referred to as the Elastic Stack server.

 ### Contents:
-  [Version](#version)
-  [Prerequisites](#)
-  [installation instructions](#)



[Version](#version)                
 
 |  Name                        |Version |
 |   -----                  |------                   |
 | Elasticksearch               | 7.6.5|
 | Kibana                         | 7.6.5|
 | Logshtash                       | 7.6.5|

[Prerequisites](#prerequisites)

Before you start with this tutorial, make sure you are logged into your server with a user account with sudo privileges or with the root user.

 - Nginx installed on your server, which you will configure later in this guide as a reverse proxy for Kibana. 
 _**Follow our guide on How To Install Nginx on CentOS 7 to set this up. 
 [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Install%20Nginx.md)**_


* Java 8 — which is required by Elasticsearch and Logstash — installed on your server. 
_**Follow our guide on How To Install Java 8 on CentOS 7 to set this up. [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Install%20Java.md)**_

- Both of the following DNS records set up for your server.

    - An A record with team3acirrustech.com pointing to your server’s public IP address.
    - An A record with www.team3acirrustech.com pointing to your server’s public IP address.


We used team3acirrustech.com domain name for our project but you can use your own domain name.


## Step 1 . [Elasticsearch](elasticsearch)
Elasticsearch is a search and analytics engine.
The open source, distributed, RESTful, JSON-based search engine. Easy to use, scalable and flexible, it earned hyper-popularity among users and a company formed around it, you know, for search.





_**Follow our guide on How To Install Nginx on CentOS 7 to set this up.[Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Install%20Elasticsearch.md)**_