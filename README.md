# Træfik 2.0 into API Platform

[Introduction](#introduction)  
[Factorization P1](#factorization-p1)  
[Multiple instances](#multiple-instances)  
[Factorization P2](#factorization-p2)  
[Scaling](#scaling)  
[Security](#security-&-ssl)  

## Introduction
|  Folder source  |  Folder solution  |
|:---:|:---:|
|[introduction](introduction)|[factorization-p1](factorization-p1)|

In this part, we are starting from API Platform plain base to 
implement functional basic traefik instance

## Factorization P1
|  Folder source  |  Folder solution  |
|:---:|:---:|
|[factorization-p1](factorization-p1)|[multiple-instances](multiple-instances)|

Take back our modifications from this part located in `factorization-p1`
folder to factorise our setup to edit less things that will be shared to
many labels

## Multiple instances
|  Folder source  |  Folder solution  |
|:---:|:---:|
|[multiple-instance](multiple-instances)|[factorization-p2](factorization-p2)|

Use only one instance of API Platform is so sad, so, in this part we will
use one shared træfik instance with many API Platform instances. It will
work with many other docker-compose duplication containers using same ports

To handle that, we will deal with networks docker-compose clusterisation.  
From now Træfik instance will only be into [external folder](external)

## Factorization P2
|  Folder source  |  Folder solution  |
|:---:|:---:|
|[factorization-p2](factorization-p2)|[scaling](scaling)|

Setup regex to resolve multiple subdomains and domains and manage 
applications from .env

## Scaling
|  Folder source  |  Folder solution  |
|:---:|:---:|
|[scaling](scaling)|[security](security)|

If you have to deploy more than one instance of one service, you should use
`scale` docker swarm command instead of duplicate your app. Then it will 
introduce a very cool thing : `load-balancing` by Træefik  
This load-balancing is incremental modulo load-balancing so, you don't have
logic behind this but in many apps it should be ok. In bigger app you should
use kubernetes orchestration to handle the load-balancing  
We will use a whoami container built by Emile Vauge, it's a lightweight
go server container then after implement it, launched it, scale it doing
```bash
docker-compose up -d --scale whoami=3
```
just curl whoami.domain.com to bypass browser cache and you'll see the IP
will load balance into 3 different IPs

## Security & SSL
|  Folder source  |  Folder solution  |
|:---:|:---:|
|[security](security)|[final](final)|

Setup fail2ban linked to Traefik and other containers and central logging.  
Then setup SSL (working on production only) using Let's Encrypt Authority X3
