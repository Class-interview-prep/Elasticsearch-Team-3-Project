## Final Step — Exploring Kibana Dashboards

Let’s look at Kibana homepage: 

![1](https://assets.digitalocean.com/articles/elastic_CentOS7_120618/Kibana_Homepage_TN.png)


Click the Discover link in the left-hand navigation bar. On the Discover page, select the predefined filebeat-* index pattern to see Filebeat data. By default, this will show you all of the log data over the last 15 minutes. You will see a histogram with log events, and some log messages below:

![](https://assets.digitalocean.com/articles/elastic_CentOS7_120618/Kibana_DiscoverPage_TN.png)

Here, you can search and browse through your logs and also customize your dashboard. At this point, though, there won’t be much in there because you are only gathering syslogs from your Elastic Stack server.

Use the left-hand panel to navigate to the Dashboard page and search for the Filebeat System dashboards. Once there, you can search for the sample dashboards that come with Filebeat’s system module.

For example, you can view detailed stats based on your syslog messages:

![](https://assets.digitalocean.com/articles/elastic_CentOS7_120618/Kibana_SyslogDashboard_TN.png)

