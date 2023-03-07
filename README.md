# swarm02

**MY URL**=> https://spcn22apache.xops.ipv9.me/

**Wakatime Project: swarm02**=> https://wakatime.com/@spcn22/projects/eqzruwyrqd


**Ref** : 

**App [apache-php], I want deploy on portainer** (App ที่ต้องการ deploy บน portainer)

>https://github.com/docker/awesome-compose/tree/master/apache-php  

**The Example for code about treafik** (ตัวอย่าง code เกี่ยวกับ traefik)

>https://github.com/pitimon/dockerswarm-inhoure/blob/main/ep04-hello-world-revProxy/hello-world-https.yml 

>https://docs.docker.com/engine/reference/builder/#cmd

Contents swarm02 (สารบัญเนื้อหา swarm02)
-----------------
No. |English Title (ชื่อเรื่องภาษาอังกฤษ)  | Thai Title (ชื่อเรื่องภาษาไทย) |
----- |----- | ----- |
1)|[Step 1 Create Folder and File for APP](https://github.com/keta410/swarm02#step-1-create-folder-and-file-for-app)|ขั้นตอนที่ 1 สร้าง Folder และ File สำหรับ APP|
2)|[Step 2 Deploy on Portainer(.xops.ipv9.me)](https://github.com/keta410/swarm02#step-2-deploy-on-portainerxopsipv9me)|ขั้นตอนที่ 2 การ Deploy ขึ้นบน Portainer(.xops.ipv9.me)|
3)|[Create Image on Portianer](https://github.com/keta410/swarm02#create-image-on-portainer)|สร้าง Image บน Portainer|
4)|[Expected Result](https://github.com/keta410/swarm02#expected-result)|ผลลัพธ์ที่คาดว่าจะได้รับ|
5)|[Working Principle in docker-compose](https://github.com/keta410/swarm02#working-principle-in-docker-compose)|หลักการทำงานใน docker-compose|
_____________________________________

### **Step 1** Create Folder and File for App
ขั้นตอนที่ 1 สร้าง Folder และ File สำหรับ App
_____________________________________

1. **Create Folder Name's => *apache-php*** (สร้าง Folder ชื่อ=> *apache-php*)

2. **Create File in folder *apache-php*** (สร้าง File ใน folder *apache-php*)

    **<ins>Stucture Tree in folder apache-php</ins>** :

        (โครงสร้างใน folder apache-php)

    ```
    .
    |__ .docker
    |    |__ docker-compose.yml
    |__ app
         |__ Dockerfile
         |__ index.php
    ```

3. **Code ```Dockerfile``` and ```index.php```**

3.1 ```Dockerfile``` 

<details><summary><ins>CLICK SHOW (Dockerfile)</ins></summary>
<p>

```ruby
    CMD ["apache2-foreground"]  #command for ..
    
    FROM builder as devs-envs   

    RUN <<EOF
    apt-get update
    apt-get install -y --no-install-recommands git
    EOF

    COPY --from==gloursdocker   /docker
    CMD ["apache-foreground"]

```

</p>
</details>

3.2 ```index.php```
```php
#webage

    '<center><h1>Hello World</h></center>'
    '<center><h5> SPCN22 </h5></center>'

//center-> aglin the text in the center (กำหนดตำแหน่งของข้อความ)
//h1,h5-> size font (ขนาดตัวอักษร)
```
**All these files are in a folder named *app*** (file ทั้งหมดนี้อยู่ใน folder ที่ชื่อว่า *app*)

4. **Code ```docker-compose.yml```** 

    4.1 ```docker-compose.yml``` 

<details><summary><ins>CLICK SHOW CODE (docker-compose.yml)</summary>
<p>

```ruby
version: '3.3'  
#Config version docker for you want (กำหนด version docker ที่เราต้องการ)

service: 
    web:    #App
        image: keta410/apache-php:v1
        #It from my image(I created in Docker Hub)
        #มาจาก image ที่สร้างเองใน Docker Hub 
        .
        networks:
            - webproxy  
        .
        .
        deploy:     
        #for upload on portainer[xops.ipv9.me] (สำหรับ upload ขึ้นบน portainer[xops.ipv9.me])
    
            replicas: 1
            labels:
                - traefik.docker.network=webproxy
                - traefik.enable=true
                - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
                - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
                - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
                - traefik.http.services.${APPNAME}.loadbalancer.server.port=80

    #Additional section (ส่วนที่เพิ่มเติม)
            resources:                  #Active resources that match the scope defined in resources and limits
                                        #(ทรัพยากรที่ใช้งานกับตัวที่ตรงกับขอบเขตที่เรากำหนดใน reseurces และ limits)
                reservations:           
                    cpus: '0.1'
                    memory: 8M
                limits:                 
                    cpus: '0.4'
                    memory: 64M
volumes:
    app:

network:
    webproxy:
        external: ture  

``` 
</p>
</details>

**<ins>Description of ```docker-compose.yml``` </ins>** :

(คำอธิบายของ ```docker-compose.yml```) 

The description of treafik setting is mostly in swarm01. The same principle applies swarm02 deploy, but there are additional resources and limits in the deploy section. These define the machines that can upload this app

(คำอธิบายเกี่ยวกับการตั้งค่า [treafik](https://github.com/keta410/swarm01#step-1-create-folder-and-file-for-app) ส่วนใหญ่ใน swarm01 จะใช้หลักเดียวกันใน swarm02 แต่มีเพิ่มเติมมาคือ resource และ limit ที่อยู่ในส่วน deploy โดยพวกนี้จะกำหนดเครื่องที่สามารถอัพ app นี้ได้)

_____________________________________
### **Step 2** Deploy on Portainer(.xops.ipv9.me)
ขั้นตอนที่ 2 Deploy ขึ้น Portainer(.xops.ipv9.me)
_____________________________________

*   **Base Deploy on Portainer from [swarm01](https://github.com/keta410/swarm01#step-2-deploy-on-portainerxopsipv9me) 
customize code base on this app**

    (อิงการ Deploy on Portainer(.xops.ipv9.me) จาก [swarm01](https://github.com/keta410/swarm01#step-2-deploy-on-portainerxopsipv9me) ปรับแต่งตัวให้สอดคล้องกับ App นี้)

_____________________________________
### **Create Image on Portainer**
สร้าง Image บน Portainer
_____________________________________

1. **Press Images** (กด Images)

2. **Click [*Build a new image*] and set name's image** (คลิก [*Build a new image*] และตั้งค่าชื่อ image) 

**<ins>Format</ins>**

```
    Github account/ application name : version

    //I set it (ที่ตั้งค่าไว้)>> keta410/apache-php:v1  
```

3. **Click [*Web editor*] and Copy code from file ```Dockerfile```**
 (คลิก [*Web editor*] และ Copy code จาก file ```Dockerfile```)

![setName-code-image](https://user-images.githubusercontent.com/104758471/222944709-0cfc5513-1a8b-4287-baf8-40a1dd208eb2.jpg)

**Then Click [*Build the image*]** (จากนั้นคลิก [*Build the image*])

![Screenshot 2023-03-02 171423](https://user-images.githubusercontent.com/104758471/222944824-1dfd5287-0adc-47ea-b6c9-bc96909c8a41.png)

______________________________________________________

### **Expected Result**
ผลลัพธ์ที่คาดว่าจะได้รับ
______________________________________________________

* **After add stack** (หลัง add stack)

* **Web browsers, I configure can access Apache-PHP** (เว็บเบราว์เซอร์ที่เรากำหนดให้สามารถเข้าถึง Apache-PHP ได้)
 
* **Create Image on portainer** (สร้าง Image บน portainer)

**When the Image is created, the following appearance will be displayed** (เมื่อสร้าง Image เสร็จจะปรากฏหน้าตาดังนี้)

![dis-image-complete](https://user-images.githubusercontent.com/104758471/222945083-e352818f-b2c5-48ed-98fe-4487b9d81520.jpg)

______________________________________________________

### **Working Principle in docker-compose**
หลักการทำงานใน docker-compose
______________________________________________________

**<ins>Under the ```docker-compose.yml``` of the app [*apache-php*] is specified</ins>** :

(ภายใต้การทำงานของ ```docker-compose.yml``` ของ app [*apache-php*] มีการระบุ)

- **Version** of the compose file directly, we can specify any version that is supported by the app we choose, in this case choose 3.3

    (**Version** ของ compose file โดยตรงนี้ เราสามารถระบุเป็น version ที่เท่าไหร่ก็ได้ที่สนับสนุนกับ app ที่เราเลือก ในที่นี้เลือก 3.3)

- **Service** specifies a container to use. Here we have web. Inside our container we have image, command, volumes, networks, environment and deploy etc.

    (**Service** ใช้ระบุ container ที่จะใช้ ในที่นี้มี web โดยภายใน container เรานี้ประกอบไปด้วย image, command, volumes, networks, environment และ deploy เป็นต้น)

- **Volumes** are used to create storage. This is different from the volumes in the service because that section is linked to the individual volumes of the created container.

    (**Volumes** ใช้สร้างที่เก็บข้อมูล ซึ่งแตกต่างจาก volumes ที่อยู่ใน service เพราะตรงในส่วนนั้นจะเป็นการเชื่อม volumes แต่ละตัว container ที่สร้างไว้)

- **Networks** are connections of existing or self-contained, networks communicating with compose files.

    (**Networks** เป็นการเชื่อมต่อกันของ network ที่มีหรือสร้างเอง สื่อสารกับ compose file)

- **Dockerfile** is used to create the image that is specified in the file. Inside the Dockerfile contains the FROM command used to load the image that we will use in the container. In this case, php:8.0.9-apache. There are commands to create the builder, CMD configurator, RUN command to run code, and COPY to set the source command parameter to be replaced with the one we define in the Dockerfile, etc. To define these commands, depends on the app to use.

    (**Dockerfile** ใช้สร้าง Image ที่กำหนดขึ้นมาในไฟล์ ภายใน Dockerfile ประกอบไปด้วยคำสั่ง FROM ใช้ในการโหลด image ที่เราจะมาใช้ใน container โดยในที่นี้คือ php:8.0.9-apache พร้อมมีคำสั่งในการสร้าง builder, CMD ตัวกำหนดค่า, RUN คำสั่งในการ run code และCOPY กำหนดให้พารามิเตอร์แหล่งที่มาคำสั่งถูกแทนที่ด้วยตัวที่เรากำหนดใน Dockerfile เป็นต้น ในการกำหนดคำสั่งเหล่านี้ก็ขึ้นอยู่กับ app ที่จะใช้ )

**<ins>Summary</ins>**

When command ```docker compose up -d``` or Click right the ```docker-compose.yml``` file, then select [**Compose Up**]. this will cause the container to be created, the network will record the data specified for each on the container, the port that is specified or a domain name that uses traefik to help manage as we define, because the apache-php has a Dockerfile that we have made in Image format and have retrieved this part in docker-compose.yml through the image inside the service, so it can be used and displayed as we specify.

(**<ins>สรุป</ins>**

เมื่อสั่ง ```docker compose up -d``` หรือคลิกขวาที่ไฟล์ ```docker-compose.yml``` เลือก [**Compose Up**] ขึ้นไปจะทำให้มีการสร้างตัว container ,network มีการบันทึกข้อมูลตามที่กำหนดไว้ในแต่ละตัวบน container, port ที่ถูกระบุใช้งาน หรือชื่อโดเมนที่มีการใช้ traefik ช่วยในการจัดการ และเนื่องจากตัวของ apache-php มี Dockerfile ที่เราได้ทำให้อยู่ในรูปแบบ Image และมีการดึงข้อมูลตรงส่วนนี้ใน docker-compose.yml ผ่าน image ข้างใน service จึงทำให้สามารถใช้งานและแสดงผลตามที่เรากำหนดได้)