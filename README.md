# Elastic Stack  Installation on CentOS 7
---





## Introduction
The Elastic Stack is a collection of open-source software produced by Elastic which allows you to search, analyze, and visualize logs generated from any source in any format, a practice known as centralized logging. 


The Elastic Stack has four main components:

1. [Elasticsearch](elasticsearch)
1. Logstash 
1. Kibana 
1. Beat


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