---
title: Linkis Console Deployment
sidebar_position: 6
---

Linkis 1.0 provides a Linkis Console, which provides functions such as displaying Linkis' global history, modifying user parameters, managing ECM and microservices, etc. Before deploying the front-end management console, you need to deploy the Linkis back-end. Linkis deployment manual See: [Linkis Deployment Manual](deployment/quick_deploy.md)

## 1. Preparation

1. Download the web installation package from the release page of Linkis ([click here to enter the download page](https://github.com/apache/incubator-linkis/releases)), apache-linkis-xxx-incubating-web-bin. tar.gz
Manually decompress: tar -xvf apache-linkis-x.x.x-incubating-web-bin.tar.gz, the decompressed directory is:
```
config.sh
dist
install.sh
....
```

## 2. Deployment
&nbsp;&nbsp;&nbsp;&nbsp;There are two deployment methods, automated deployment and manual deployment

### 2.1 Automated deployment
&nbsp;&nbsp;&nbsp;&nbsp;Enter the unzipped front-end directory, and edit ```vi config.sh ``` in the directory
Change the front-end port and back-end interface address, the back-end interface address is the gateway address of Linkis.

```$xslt
#Configuring front-end ports
linkis_port="8088"

#URL of the backend linkis gateway
linkis_url="http://localhost:9001"

#linkis ip address
linkis_ipaddr=$(ip addr | awk'/^[0-9]+: / {}; /inet.*global/ {print gensub(/(.*)\/(.*)/, "\\1" , "g", $2)}')
```

After the modification is executed in this directory, you need to use sudo to execute: ```sudo sh install.sh ```

After execution, you can directly access it in Google browser: ```http://linkis_ipaddr:linkis_port``` where linkis_port is the port configured in config.sh, and linkis_ipaddr is the IP of the installation machine

If the access fails: You can check the installation log which step went wrong

### 2.2 Manual deployment
1. Install Nginx: ```sudo yum install nginx -y```

2. Modify the configuration file: sudo vi /etc/nginx/conf.d/linkis.conf
Add the following content:
```
server {
            listen 8080;# access port
            server_name localhost;
            #charset koi8-r;
            #access_log /var/log/nginx/host.access.log main;
            location / {
            root /appcom/Install/linkis/dist; # The directory where the front-end package is decompressed
            index index.html index.html;
            }
          
            location /api {
            proxy_pass http://192.168.xxx.xxx:9001; # ip port of linkis-gateway service
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header x_real_ipP $remote_addr;
            proxy_set_header remote_addr $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_http_version 1.1;
            proxy_connect_timeout 4s;
            proxy_read_timeout 600s;
            proxy_send_timeout 12s;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection upgrade;
            }
            #error_page 404 /404.html;
            # redirect server error pages to the static page /50x.html
            #
            error_page 500 502 503 504 /50x.html;
            location = /50x.html {
            root /usr/share/nginx/html;
            }
        }

```

3. Copy the front-end package to the corresponding directory: ```/appcom/Install/linkis/dist; # The directory where the front-end package is decompressed ```

4. Start the service ```sudo systemctl restart nginx```

5. After execution, you can directly access it in Google browser: ```http://nginx_ip:nginx_port```

## 3. Common problems

(1) Upload file size limit

```
sudo vi /etc/nginx/nginx.conf
```

Change upload size

```
client_max_body_size 200m
```

 (2) Interface timeout

```
sudo vi /etc/nginx/conf.d/linkis.conf
```


Change interface timeout

```
proxy_read_timeout 600s
```