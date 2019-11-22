---
layout: post
title:  "Generate Trusted Certificate on AWS AMI"
author: tal
categories: [ dev ]
tags: [ami, amazon, certificate]
image: assets/images/20.jpg
description: "This is the ground control station for the drone, this one contain the GUI logic (based on JFX2). This clients communicate with the drone server to get and store data. It is also work with the map viewer project to get map access, image processing project to have FPV and generic tools to get access to USB devices."
featured: false
hidden: false
rating: 4.5
---

The following directory hold the DroneGCS project files for the GUI and Controller components. It consist of code, drivers and executable jars directory. In order to have a better understanding of the code simply dig into it. The controller components exist in the GUI Plugin is responsible of two things, the first, communicate with the drone itself, running mission, flight control and many other nice utility. It second hat is to communicate with the server out there in the cloud, the server responsible of saving you configuration, saving your mission and perimeters. The server gives you a private DB to work on until you save it to the public DB.

Let’s Encrypt is a free, automated, and open certificate authority (CA), run for the public’s benefit. It is a service provided by the Internet Security Research Group (ISRG). It give people the digital certificates they need in order to enable HTTPS (SSL/TLS) for websites, for free, in the most user-friendly way we can. We do this because we want to create a more secure and privacy-respecting Web.

#### Install Python
$ yum install python27-devel git

#### Create a file for nginx
$ touch /etc/nginx/conf.d/myplayground.us-east-2.elasticbeanstalk.com.conf
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
