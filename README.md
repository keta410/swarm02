# swarm02

### Create Folder for App
1. Create Folder Name's => *apache-php*
2. Create File in folder *apache-php*

    Stucture Tree in folder apache-php :    
    
    *  .docker
        * docker-compose.yml
    * app
        * Dockerfile
        * index.php


3. Code in  ```docker-compose.yml``` for apache-php 

```docker
version: '3.3'  
#Version Docker for you want
service: 
    web:    
    #Name of app
        image: keta410/apache-php:v1
    #I change for my image(I created in Docker Hub)
    .
    networks:
        - webproxy  
        #Name's network
    .
    .
    deploy:     
    #for install on portainer(xops.ipv9.me)
    
        replicas: 1
        labels:
            - traefik.docker.network=webproxy
            - traefik.enable=true
            - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
            - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
            - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
            - traefik.http.services.${APPNAME}.loadbalancer.server.port=80

        resources:
            reservations:
                cpus: '0.1'
                memory: 8M
            limits:
                cpus: '0.4'
                memory: 64M
volumes:
    .
network:
    webproxy:
        external: ture  
        #Config, it can used only in webproxy network

``` 


Ref : 
>https://github.com/docker/awesome-compose/tree/master/apache-php  

App, I want deploy on portainer

>https://github.com/pitimon/dockerswarm-inhoure/blob/main/ep04-hello-world-revProxy/hello-world-https.yml 

The Example for code about treafik