# swarm02

**MY URL**=> https://spcn22apache.xops.ipv9.me/

**Wakatime Project: swarm02**=> https://wakatime.com/@spcn22/projects/eqzruwyrqd

**Ref** : 
>https://github.com/docker/awesome-compose/tree/master/apache-php  

App, I want deploy on portainer

>https://github.com/pitimon/dockerswarm-inhoure/blob/main/ep04-hello-world-revProxy/hello-world-https.yml 

The Example for code about treafik
_____________________________________

### **Step 1** Create Folder and File for App
**ขั้นตอนที่ 1** สร้าง Folder และ File สำหรับ App
_____________________________________

1. Create Folder Name's => **apache-php**
    
    สร้าง Folder ชื่อ=> **apache-php**

2. Create File in folder *apache-php*

    สร้าง File ใน folder *apache-php*

    <ins>Stucture Tree in folder apache-php</ins> :

        โครงสร้างใน folder apache-php :

    ```
    .
    |__ .docker
    |    |__ docker-compose.yml
    |__ app
         |__ Dockerfile
         |__ index.php
    ```

3. Code ```Dockerfile``` and ```index.php```

3.1 ```Dockerfile``` 

```dockerfile
    CMD ["apache2-foreground"]  #command for ..
    
    FROM builder as devs-envs   

    RUN <<EOF
    apt-get update
    apt-get install -y --no-install-recommands git
    EOF

    COPY --from==gloursdocker   /docker
    CMD ["apache-foreground"]

```

3.2```index.php```
```php
#webage

    '<center><h1>Hello World</h></center>'
    '<center><h5> SPCN22 </h5></center>'

//center-> aglin the text in the center (กำหนดตำแหน่งของข้อความ)
//h1,h5-> size font (ขนาดตัวอักษร)
```
All these files are in a folder named **app**

4. Create Image on Portainer

4.1 Press Images

    กด Images

4.2 Click [**Build a new image**] and set name's image

<ins>Format</ins>

```
    Github account/ application name : version

    //I set it (ที่ตั้งค่าไว้)>> keta410/apache-php:v1  
```

4.3 Click [**Web editor**] and Copy code from file ```Dockerfile```

คลิก [**Web editor**] และ Copy code จาก file ```Dockerfile```

![setName-code-image](https://user-images.githubusercontent.com/104758471/222944709-0cfc5513-1a8b-4287-baf8-40a1dd208eb2.jpg)

Then Click [**Build the image**]

จากนั้นคลิก [**Build the image**]

![Screenshot 2023-03-02 171423](https://user-images.githubusercontent.com/104758471/222944824-1dfd5287-0adc-47ea-b6c9-bc96909c8a41.png)

When the Image is created, the following appearance will be displayed

เมื่อสร้าง Image เสร็จจะปรากฏหน้าตาดังนี้

![dis-image-complete](https://user-images.githubusercontent.com/104758471/222945083-e352818f-b2c5-48ed-98fe-4487b9d81520.jpg)

5. Code ```docker-compose.yml``` 

    5.1 ```docker-compose.yml``` ()

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

``` 
Full code in ```docker-compose.yml```
>(website)

_____________________________________

### **Step ** Deploy on Portainer(.xops.ipv9.me)
**ขั้นตอนที่ ** Deploy ขึ้น Portainer(.xops.ipv9.me)
_____________________________________

