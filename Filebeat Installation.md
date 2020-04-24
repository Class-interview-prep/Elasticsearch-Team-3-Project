# Step 4 — Installing and Configuring Filebeat

What is Filebeat?

The Elastic Stack uses several lightweight data shippers called Beats to collect data from various sources and transport them to Logstash or Elasticsearch. Here are the Beats that are currently available from Elastic:

- Filebeat: collects and ships log files.

- Metricbeat: collects metrics from your systems and services.

- Packetbeat: collects and analyzes network data.

- Winlogbeat: collects Windows event logs.

- Auditbeat: collects Linux audit framework data and monitors file integrity.

- Heartbeat: monitors services for their availability with active probing.

In this tutorial, we will use Filebeat to forward local logs to our Elastic Stack.

Install Filebeat using yum command:
```
           sudo yum install filebeat

```
Next, configure Filebeat to connect to Logstash. Here, we will modify the example configuration file that comes with Filebeat.

Open the Filebeat configuration file:
```
        sudo vi /etc/filebeat/filebeat.yml
```
Filebeat supports numerous outputs, but you’ll usually only send events directly to Elasticsearch or to Logstash for additional processing. In this tutorial, we’ll use Logstash to perform additional processing on the data collected by Filebeat. Filebeat will not need to send any data directly to Elasticsearch, so let’s disable that output. To do so, find the output.elasticsearch section and comment out the following lines by preceding them with a #:
```
...
#output.elasticsearch:
  # Array of hosts to connect to.
  #hosts: ["localhost:9200"]
...

```
Then, configure the output.logstash section. Uncomment the lines output.logstash: and hosts: ["localhost:5044"] by removing the #. This will configure Filebeat to connect to Logstash on your Elastic Stack server at port 5044, the port for which we specified a Logstash input earlier:
```
output.logstash:
  # The Logstash hosts
  hosts: ["localhost:5044"]

```
-    - Note: As with Elasticsearch, Filebeat’s configuration file is in YAML format. This means that proper indentation is crucial, so be sure to use the same number of spaces that are indicated in these instructions.



You can now extend the functionality of Filebeat with Filebeat modules. In this tutorial, you will use the system module, which collects and parses logs created by the system logging service of common Linux distributions.

Let’s enable it:

```
      sudo filebeat modules enable system
```


You can see a list of enabled and disabled modules by running:
```
            sudo filebeat modules list
```

You will see a list similar to the following:

```
Output
Enabled:
system

Disabled:
apache2
auditd
elasticsearch
haproxy
icinga
iis
kafka
kibana
logstash
mongodb
mysql
nginx
osquery
postgresql
redis
suricata
traefik
```


By default, Filebeat is configured to use default paths for the syslog and authorization logs. In the case of this tutorial, you do not need to change anything in the configuration. You can see the parameters of the module in the /etc/filebeat/modules.d/system.yml configuration file.

Next, load the index template into Elasticsearch. An Elasticsearch index is a collection of documents that have similar characteristics. Indexes are identified with a name, which is used to refer to the index when performing various operations within it. The index template will be automatically applied when a new index is created.

To load the template, use the following command:
```
sudo filebeat setup --template -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'
```

This will give the following output:

```
Output
Loaded index template

```
Filebeat comes packaged with sample Kibana dashboards that allow you to visualize Filebeat data in Kibana. Before you can use the dashboards, you need to create the index pattern and load the dashboards into Kibana.

As the dashboards load, Filebeat connects to Elasticsearch to check version information. To load dashboards when Logstash is enabled, you need to manually disable the Logstash output and enable Elasticsearch output:
```
sudo filebeat setup -e -E output.logstash.enabled=false -E output.elasticsearch.hosts=['localhost:9200'] -E setup.kibana.host=localhost:5601
```

You will see output that looks like this:
```
Output
. . .
2018-12-05T21:23:33.806Z        INFO    elasticsearch/client.go:163     Elasticsearch url: http://localhost:9200
2018-12-05T21:23:33.811Z        INFO    elasticsearch/client.go:712     Connected to Elasticsearch version 6.5.2
2018-12-05T21:23:33.815Z        INFO    template/load.go:129    Template already exists and will not be overwritten.
Loaded index template
Loading dashboards (Kibana must be running and reachable)
2018-12-05T21:23:33.816Z        INFO    elasticsearch/client.go:163     Elasticsearch url: http://localhost:9200
2018-12-05T21:23:33.819Z        INFO    elasticsearch/client.go:712     Connected to Elasticsearch version 6.5.2
2018-12-05T21:23:33.819Z        INFO    kibana/client.go:118    Kibana url: http://localhost:5601
2018-12-05T21:24:03.981Z        INFO    instance/beat.go:717    Kibana dashboards successfully loaded.
Loaded dashboards
2018-12-05T21:24:03.982Z        INFO    elasticsearch/client.go:163     Elasticsearch url: http://localhost:9200
2018-12-05T21:24:03.984Z        INFO    elasticsearch/client.go:712     Connected to Elasticsearch version 6.5.2
2018-12-05T21:24:03.984Z        INFO    kibana/client.go:118    Kibana url: http://localhost:5601
2018-12-05T21:24:04.043Z        WARN    fileset/modules.go:388  X-Pack Machine Learning is not enabled
2018-12-05T21:24:04.080Z        WARN    fileset/modules.go:388  X-Pack Machine Learning is not enabled
Loaded machine learning job configurations
```

Now you can start and enable Filebeat:

```
        sudo systemctl start filebeat
        sudo systemctl enable filebeat

```

If you’ve set up your Elastic Stack correctly, Filebeat will begin shipping your syslog and authorization logs to Logstash, which will then load that data into Elasticsearch.

To verify that Elasticsearch is indeed receiving this data, query the Filebeat index with this command:

```
curl -X GET 'http://localhost:9200/filebeat-*/_search?pretty'
```
You will see an output that looks similar to this:

```
Output
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 3,
    "successful" : 3,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 3225,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "filebeat-6.5.2-2018.12.05",
        "_type" : "doc",
        "_id" : "vf5GgGcB_g3p-PRo_QOw",
        "_score" : 1.0,
        "_source" : {
          "@timestamp" : "2018-12-05T19:00:34.000Z",
          "source" : "/var/log/secure",
          "meta" : {
            "cloud" : {
. . .
```

If your output shows 0 total hits, Elasticsearch is not loading any logs under the index you searched for, and you will need to review your setup for errors. If you received the expected output you are good to go for last step.


Whalla, Installation is done! In a web browser, enter your public IP address of your Elastic Stack server. After entering the login credentials you defined in Step 2,

  You will see :

![Kibana Web](https://assets.digitalocean.com/articles/elastic_CentOS7_120618/Kibana_Homepage_TN.png)

Let's take our last step and Exploring Kibana Dashboards!!! [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Kibana%20Installation.md)