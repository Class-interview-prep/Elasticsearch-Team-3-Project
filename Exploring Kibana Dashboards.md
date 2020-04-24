## Final Step — Exploring Kibana Dashboards

Let’s look at Kibana homepage: 

![1](https://assets.digitalocean.com/articles/elastic_CentOS7_120618/Kibana_Homepage_TN.png)


Click the Discover link in the left-hand navigation bar. On the Discover page, select the predefined filebeat-* index pattern to see Filebeat data. By default, this will show you all of the log data over the last 15 minutes. You will see a histogram with log events, and some log messages below:

![](https://assets.digitalocean.com/articles/elastic_CentOS7_120618/Kibana_DiscoverPage_TN.png)

Here, you can search and browse through your logs and also customize your dashboard. At this point, though, there won’t be much in there because you are only gathering syslogs from your Elastic Stack server.

Use the left-hand panel to navigate to the Dashboard page and search for the Filebeat System dashboards. Once there, you can search for the sample dashboards that come with Filebeat’s system module.

For example, you can view detailed stats based on your syslog messages:

![](https://assets.digitalocean.com/articles/elastic_CentOS7_120618/Kibana_SyslogDashboard_TN.png)

You can also view which users have used the sudo command and when:

![](https://assets.digitalocean.com/articles/elastic_CentOS7_120618/Kibana_Sudo_Page_TN.png)

 
Kibana has many other features, such as graphing and filtering, so feel free to explore.

Conclusion:

In this tutorial, you installed and configured the Elastic Stack to collect and analyze system logs. Remember that you can send just about any type of log or indexed data to Logstash using Beats, but the data becomes even more useful if it is parsed and structured with a Logstash filter, as this transforms the data into a consistent format that can be read easily by Elasticsearch.

Sources:

 <https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-centos-7>

 <https://guides.github.com/features/wikis/>

 <https://www.elastic.co/elasticsearch/service?ultron=[EL]-[B]-[AMER]-US+CA-BMM&blade=adwords-s&Device=c&thor=%2Belasticsearch%20%2Binstall&gclid=CjwKCAjwnIr1BRAWEiwA6GpwNWyCCIKb6aH049pEtt7LlSFk2xD05gn12UNPn0DZW2F9qvp6wJUCLhoCMxgQAvD_BwE>