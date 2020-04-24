# Step 2 — Installing and Configuring the Kibana Dashboard
By default, Kibana listens on port 5601.
What is Kibana?
Kibana is an open source frontend application that sits on top of the Elastic Stack, providing search and data visualization capabilities for data indexed in Elasticsearch. Commonly known as the charting tool for the Elastic Stack (previously referred to as the ELK Stack after Elasticsearch, Logstash, and Kibana), Kibana also acts as the user interface for monitoring, managing, and securing an Elastic Stack cluster — as well as the centralized hub for built-in solutions developed on the Elastic Stack. 

After setting Kibana up, we will be able to use its interface to search through and visualize the data that Elasticsearch stores.

Because you already added the Elastic repository in the previous step, you can just install the remaining components of the Elastic Stack using yum:

```
          sudo yum install kibana
```

Enable and start the Kibana service:
```
            sudo systemctl enable kibana
            sudo systemctl start kibana
 ```
 Because Kibana is configured to only listen on localhost, we must set up a reverse proxy to allow external access to it. We will use Nginx for this purpose, which should already be installed on your server.

 First, use the openssl command to create an administrative Kibana user which you’ll use to access the Kibana web interface. As an example, we will name this account kibanaadmin but you can chose your own user name the following command will create the administrative Kibana user and password, and store them in the htpasswd.users file.
 ```
echo "kibanaadmin:`openssl passwd -apr1`" | sudo tee -a /etc/nginx/htpasswd.users
```

 - Remember or take note of this login, as you will need it to access the Kibana web interface.


Next, we will create an Nginx server block file. As an example, we will refer to this file as example.com.conf. You can chose your own name.
```
sudo vi /etc/nginx/conf.d/team3.acirrustech.com.conf
```


```
server {
    listen 80;

    server_name team3.acirrustech.com www.team3.acirrustech;

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
-  - Make sure to update team3.acirrustech.com  and www.team3.acirrustech.com to match your server’s public IP address. This code configures Nginx to direct your server’s HTTP traffic to the Kibana application, which is listening on localhost:5601. Additionally, it configures Nginx to read the htpasswd.users file and require basic authentication.

Then check the configuration for syntax errors:

```
                sudo nginx -t
```

-  - If any errors are reported in your output, go back and double check that the content you placed in your configuration file was added correctly. Once you see syntax is ok in the output, go ahead and restart the Nginx service. 

```
         sudo systemctl restart nginx
```
#Run the following command to allow Nginx to access the proxied service:
```
   sudo setsebool httpd_can_network_connect 1 -P
```

Kibana is now accessible via your FQDN or the public IP address of your Elastic Stack server. You can check the Kibana server’s status page by navigating to the following address and entering your login credentials when prompted:
```
http://your_server_ip/status
```
Congrats!!! Kibana dashboard is configured and Kibana status is Green!

![Kibana](https://i.imgur.com/QSiMhUD.png)

-    - This status page displays information about the server’s resource usage and lists the installed plugins.

Next, [Click Here](https://github.com/solongocyber/Elasticsearch-Team-3-Project/blob/master/Logstash%20Installation.md)