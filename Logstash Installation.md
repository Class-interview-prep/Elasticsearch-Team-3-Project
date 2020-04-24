# Step 3 — Installing and Configuring Logstash

Whta is Logstash?


![Logstash logo](https://www.javainuse.com/beats-logstash.jpg)

Logstash is an open source data collection engine with real-time pipelining capabilities. Logstash will allow you to collect data from different sources, transform it into a common format, and export it to another database. Logstash’s configuration files are written in the JSON format and reside in the /etc/logstash/conf.d directory. 
The Logstash event processing pipeline has three stages: 

Inputs → Filters → Outputs.

Inputs generate events, filters modify them, and outputs ship them elsewhere. 

![Logstash](https://assets.digitalocean.com/articles/elastic_1804/logstash_pipeline_updated.png)



Install Logstash with this command:
```
         sudo yum install logstash
```

Create a configuration file called 02-beats-input.conf where you will set up your Filebeat input:

```
    sudo vi /etc/logstash/conf.d/02-beats-input.conf
```

Insert the following input configuration. This specifies a beats input that will listen on TCP port 5044.
```

input {
  beats {
    port => 5044
  }
}


```

Create a configuration file called 10-syslog-filter.conf, which will add a filter for system logs, also known as syslogs:
```
sudo vi /etc/logstash/conf.d/10-syslog-filter.conf
```

Insert the following syslog filter configuration. This example system logs configuration was taken from official Elastic documentation. This filter is used to parse incoming system logs to make them structured and usable by the predefined Kibana dashboards:

```
filter {
  if [fileset][module] == "system" {
    if [fileset][name] == "auth" {
      grok {
        match => { "message" => ["%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} %{DATA:[system][auth][ssh][method]} for (invalid user )?%{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]} port %{NUMBER:[system][auth][ssh][port]} ssh2(: %{GREEDYDATA:[system][auth][ssh][signature]})?",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} user %{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: Did not receive identification string from %{IPORHOST:[system][auth][ssh][dropped_ip]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sudo(?:\[%{POSINT:[system][auth][pid]}\])?: \s*%{DATA:[system][auth][user]} :( %{DATA:[system][auth][sudo][error]} ;)? TTY=%{DATA:[system][auth][sudo][tty]} ; PWD=%{DATA:[system][auth][sudo][pwd]} ; USER=%{DATA:[system][auth][sudo][user]} ; COMMAND=%{GREEDYDATA:[system][auth][sudo][command]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} groupadd(?:\[%{POSINT:[system][auth][pid]}\])?: new group: name=%{DATA:system.auth.groupadd.name}, GID=%{NUMBER:system.auth.groupadd.gid}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} useradd(?:\[%{POSINT:[system][auth][pid]}\])?: new user: name=%{DATA:[system][auth][user][add][name]}, UID=%{NUMBER:[system][auth][user][add][uid]}, GID=%{NUMBER:[system][auth][user][add][gid]}, home=%{DATA:[system][auth][user][add][home]}, shell=%{DATA:[system][auth][user][add][shell]}$",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} %{DATA:[system][auth][program]}(?:\[%{POSINT:[system][auth][pid]}\])?: %{GREEDYMULTILINE:[system][auth][message]}"] }
        pattern_definitions => {
          "GREEDYMULTILINE"=> "(.|\n)*"
        }
        remove_field => "message"
      }
      date {
        match => [ "[system][auth][timestamp]", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
      }
      geoip {
        source => "[system][auth][ssh][ip]"
        target => "[system][auth][ssh][geoip]"
      }
    }
    else if [fileset][name] == "syslog" {
      grok {
        match => { "message" => ["%{SYSLOGTIMESTAMP:[system][syslog][timestamp]} %{SYSLOGHOST:[system][syslog][hostname]} %{DATA:[system][syslog][program]}(?:\[%{POSINT:[system][syslog][pid]}\])?: %{GREEDYMULTILINE:[system][syslog][message]}"] }
        pattern_definitions => { "GREEDYMULTILINE" => "(.|\n)*" }
        remove_field => "message"
      }
      date {
        match => [ "[system][syslog][timestamp]", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
      }
    }
  }
}

```

Lastly, create a configuration file called 30-elasticsearch-output.conf:

```
sudo vi /etc/logstash/conf.d/30-elasticsearch-output.conf
```
Insert the following output configuration. This output configures Logstash to store the Beats data in Elasticsearch, which is running at localhost:9200, in an index named after the Beat used. The Beat used in this tutorial is Filebeat:
```
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    manage_template => false
    index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
  }
}
```

-    - If you want to add filters for other applications that use the Filebeat input, be sure to name the files so they’re sorted between the input and the output configuration, meaning that the file names should begin with a two-digit number between 02 and 30.



Test your Logstash configuration with this command:
```
sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
```


If there are no syntax errors, your output will display Configruation OK after a few seconds. If you don’t see this in your output, check for any errors that appear in your output and update your configuration to correct them.

Once your configuration test is successful, start and enable Logstash: 
```
                sudo systemctl start logstash
                sudo systemctl enable logstash
```


https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-centos-7#step-5-%E2%80%94-exploring-kibana-dashboards