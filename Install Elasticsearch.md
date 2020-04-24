# Step-1 : Installing and Configuring Elasticsearch

All of the Elastic Stack’s packages are signed with the Elasticsearch signing key in order to protect your system from package spoofing. Packages which have been authenticated using the key will be considered trusted by your package manager. In this step, you will import the Elasticsearch public GPG key and add the Elastic package source list in order to install Elasticsearch.

To begin, run the following command to import the Elasticsearch public GPG key into APT:
```
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

 Add the Elastic repository. Use your preferred text editor to create the file elasticsearch.repo in the /etc/yum.repos.d/ directory. Here, we’ll use the vi text editor:
```
sudo vi /etc/yum.repos.d/elasticsearch.repo
```
 - - Please append following: 
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

 Install Elasticsearch with the following command:
```
sudo yum install elasticsearch
```
 Once Elasticsearch is finished installing, open its main configuration file, elasticsearch.yml, in your editor:

```
sudo vi /etc/elasticsearch/elasticsearch.yml
```
 Elasticsearch listens for traffic from everywhere on port 9200. You will want to restrict outside access to your Elasticsearch instance to prevent outsiders from reading your data or shutting down your Elasticsearch cluster through the REST API. Find the line that specifies network.host, uncomment it, and replace its value with localhost so it looks like this:

```
. . .
network.host: localhost
. . .
```

 Start and Enable the Elasticsearch service with systemctl:
```
   sudo systemctl start elasticsearch
   sudo systemctl enable elasticsearch
```

You can test whether your Elasticsearch service is running by sending an HTTP request:

```
curl -X GET "localhost:9200"
```

You will see a response showing some basic information about your local node, similar to this:

```

Output
{
  "name" : "8oSCBFJ",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "1Nf9ZymBQaOWKpMRBfisog",
  "version" : {
    "number" : "6.5.2",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "9434bed",
    "build_date" : "2018-11-29T23:58:20.891072Z",
    "build_snapshot" : false,
    "lucene_version" : "7.5.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}

```
If you see this output , Elasticsearch is up and running. 

Step 2 — Installing and Configuring the Kibana Dashboard [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Kibana%20Installation.md)