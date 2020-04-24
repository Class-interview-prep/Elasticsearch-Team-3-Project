# Elastic Stack  Installation on CentOS 7
---




## Introduction
 ### What is ELK Stack ?
The Elastic Stack is a collection of open-source software produced by Elastic which allows you to search, analyze, and visualize logs generated from any source in any format, a practice known as centralized logging. 

![](https://communities.bmc.com/servlet/JiveServlet/showImage/38-12697-490504/pastedImage_9.png)

The Elastic Stack has four main components:

1 . `Elasticsearch` - Elasticsearch is a search and analytics engine. The open source, distributed,RESTful, JSON-based search engine. Easy to use, scalable and flexible, it earned hyper-popularity among users and a company formed around it, you 
know, for search.

2 . `Logstash` - Logstash is a light-weight, open-source, server-side data processing pipeline that allows you to collect data from a variety of sources, transform it on the fly, and send it to your desired destination.sh it is more flexible to do it.

3 . `Kibana` - Kibana lets you visualize your Elasticsearch data and navigate the Elastic Stack so you can do anything from tracking query load to understanding the way requests flow through your apps.

4 . `Beat` -  Beats are open source data shippers that you install as agents on your servers to send operational data to Elasticsearch.

This tutorial provides you how to install the Elastic Stack on a CentOS 7 server. At the end of this tutorial, you will have all of these components 
installed on a single server, referred to as the Elastic Stack server.

 ### Contents:

-  Versions

-  Prerequisites

-  Installation instructions



Versions :            
 
 |  Name                        |Version |
 |   -----                  |------                   |
 | Elasticksearch               | 6.8.8 |
 | Kibana                         | 6.8.8 |
 | Logshtash                       | 6.8.8 |
 | Filebeat                        |6.8.8 |


   -  - Note: When installing the Elastic Stack, you should use the same version across the entire stack. In this project we used Versions are listed above. 
 
## Prerequisites:

For this project , we created image in South America (São Paulo) region and lunched our instance in US East (N. Virginia)us-east-1 since we had to use VPC prepared by Team 1. We lunched the instance with the following specifications for our Elastic Stack server:

OS: CentOS 7

RAM: 4GB

CPU: 2

Before you start with this tutorial, make sure you are logged into your server with a user with sudo privileges or with the root user.

 - Nginx installed on your server, which you will configure later in this guide as a reverse proxy for Kibana. 

    - _**Follow our guide on How To Install Nginx 
     ( Nginx version: nginx/1.17.10 ) on CentOS 7 to 
     set this up.  [Click here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Install%20Nginx.md)**_


* Java 8 — which is required by Elasticsearch and Logstash  installed on your server. 

   -  _**Follow our guide on How To Install Java 8 ( Open JDK 8 version “1.8.0_242” ) on CentOS 7 to set this up. [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Install%20Java.md)**_

- Both of the following DNS records set up for your server.

    - An A record with team3acirrustech.com pointing to your server’s public IP address.
   
    - An A record with www.team3acirrustech.com pointing to your server’s public IP address.


We used team3acirrustech.com domain name for our project but you can use your own domain name.


## Step 1 .  [Elasticsearch](#elasticsearch)

![](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcT53B3UvRi1KoGwNQMRw_0slL-xWX3Mu70O49_yks3HzL_f_eLy&usqp=CAU)



_**Follow our guide on How To Install Elasticsearch on CentOS 7. [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Install%20Elasticsearch.md)**_



## Step 2. Kibana

![](https://user-images.githubusercontent.com/567298/55418811-b8b54c00-5573-11e9-810d-d244d27c4fb3.png)



_**Follow our guide on How To Install Kibana on CentOS 7. [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Kibana%20Installation.md)**_


## Step 3. Logstash

![Logstash logo](https://www.javainuse.com/beats-logstash.jpg)


_**Follow our guide on How To Install Logstash on CentOS 7. [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Logstash%20Installation.md)**_

## Step 4 — Installing and Configuring Filebeat

![](https://cezarypiatek.github.io/post/demystifying-elk-stack/elk_overview.jpg)

_**Follow our guide on How To Install Filebeat on CentOS 7. [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Filebeat%20Installation.md)**_