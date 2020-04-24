# How to install Nginx on CentOS 7

Nginx [engine x] is free and open source high-performance web server. It also acts as a reverse proxy server, as well as. This page shows how to install Nginx server on a CentOS 7 step by step:

- Login to your cloud server using ssh command:
``` 
       ssh centos@1.1.1.1.1
 ```

- Create the file named /etc/yum.repos.d/nginx.repo using a text editor such as vim command: 
    
```
    sudo vi /etc/yum.repos.d/nginx.repo 
```
   
and append following :
```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
gpgcheck=0
enabled=1
```
- Install nginx package using the yum command:

 ```
      sudo yum install nginx
 ```
- First enable and start nginx service by running systemctl command: 
```
     sudo systemctl enable nginx
     sudo systemctl start nginx
```


- Find status of Nginx server command:
```
    sudo systemctl status nginx
```
- Order to Open port 80 and 443 using firewall-cmd please check in your system if Firewalld is installed by default on CentOS 7, but if it is not installed on your system, you can install the package here:

```
      sudo yum install firewalld
```
- Firewalld service is disabled by default. You can check the firewall status with:
```
       sudo firewall-cmd --state
 ```
- To start the FirewallD service and enable it on boot type:
```
sudo systemctl start firewalld
sudo systemctl enable firewalld
```


- You must open and enable port 80 and 443 using the firewall-cmd command:
```
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
 sudo firewall-cmd --reload
```

- Verify that port 80 or 443 opened using ss command:
```
        sudo ss -tulpn
```
Check with IP address on the browser :

![team3](https://www.cyberciti.biz/media/new/faq/2018/01/Welcome-to-Nginx.jpg) 

