---
layout: post
title:  "Generate Trusted Certificate on AWS AMI"
author: tal
categories: [ dev ]
tags: [ami, amazon, certificate]
image: assets/images/20.jpg
description: "A detailed guidelines of how to install Let's Encrypt certificate on your AMI"
featured: false
hidden: false
rating: 4.5
---

In this article, we will install the free SSL certificate on your site which is running on Amazon AMI.
I’m assuming that you are running NGINX on an Amazon Linux EC2 instance. We’ll install a free SSL certificate from Let’s Encrypt.

Let’s Encrypt is a free, automated, and open certificate authority (CA), run for the public’s benefit. It is a service provided by the Internet Security Research Group (ISRG). It give people the digital certificates they need in order to enable HTTPS (SSL/TLS) for websites, for free, in the most user-friendly way possible.

#### Basic Requirements
First SSH to your AMI server, than, install Python
```
$ yum install python27-devel git
```

#### Create a file for nginx
```
$ touch /etc/nginx/conf.d/myplayground.us-east-2.elasticbeanstalk.com.conf
```
Then, add the following content to the file:
```
server {
    server_name myplayground.us-east-2.elasticbeanstalk.com;

    location / {
        proxy_pass https://127.0.0.1:4000;
    }
}
```

#### Install Let's Encrypt
Install Let’s Encrypt by cloning the github repository into /opt/letsencrypt and running the Let’s Encrypt installer:
```
$ git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
$ /opt/letsencrypt/letsencrypt-auto --debug
```

#### Restart nginx
```
$ killall nginx
$ /usr/sbin/nginx -c /etc/nginx/nginx.conf
```
In the above example the server address is: myplayground.us-east-2.elasticbeanstalk.com And there is an internal process that works listen to port 4000.

#### Certification Renewal
```
$ /opt/letsencrypt/letsencrypt-auto certonly --debug --webroot -w /var/www/playground -d myplayground.us-east-2.elasticbeanstalk.com -d www.myplayground.us-east-2.elasticbeanstalk.com --config /etc/letsencrypt/config.ini --agree-tos
```
Or
```
$ ./certbot-auto renew -v --debug
```
