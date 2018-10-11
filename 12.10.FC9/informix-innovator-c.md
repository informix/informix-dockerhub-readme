 
![Image of Informix Logo](https://s3.amazonaws.com/ibm.products.us-east-1/informix/informix-logo.jpg)

### What is  IBM Informix Innovator-C ?

```IBM Informix Innovator-C```  eliminates the up-front licensing costs of developing popular database functionality for use in production (redistribution requires a separate license). The Innovator-C Edition is available on Linux, Windows and Mac platforms and is limited to one core and 2 GB of memory.

Informix Innovator-C Edition delivers the following features and benefits:

* __Autonomic features__ minimize system failures and improve performance..
* __Automated backup and restore__ eliminates many manual tasks.
* __Selected support at Elite level__ is available as an optional purchase.

>[IBM Informix Family](http://www-03.ibm.com/software/products/en/informix-family)

>[IBM Informix Innovator-C Edition ](http://www-03.ibm.com/software/products/en/infoinnoedit)

>[IBM Elite Support](http://www-01.ibm.com/support/docview.wss?rs=630&uid=swg21431136)


### Supported tags

*  ```latest``` 

The supported tags stands for ```<informix version> - <Linux kernel version of Docker Engine>```.
Informix Docker images can be deployed on a Docker Engine with any flavour of Linux , as long as it has the compatible Linux  ```kernel-2.6.32```, like ```CentOS 6.6```.

* Please make sure your Docker Engine has a compatible Linux kernel
* Please use ```Docker 1.6.0 or later release``` on Ubuntu Docker Engine or Boot2Docker since there is a known issue with [aufs and direct io] (https://github.com/docker/docker/pull/10534)

### **How to use this image** ?
This docker image has to be deployed to Docker Engine on one of supported Cloud providers or your own system. The instructions for creating [Docker Engine](https://www.docker.com/whatisdocker/) vary by Cloud provider. We recommend to use [Docker Machine (beta)] (https://docs.docker.com/machine/)  to provision Docker Engine.

In order to use the image, it is necessary to accept the terms of the Informix Innovator-C license. This is achieved by specifying the environment variable LICENSE equal to accept when running the image.

This docker image contained pre-deployed Informix Innovator-C.

1 - Starting an Informix Docker Container for the First time.

```shell
docker run -it --name iif_innovator_c --privileged -p 9088:9088 -p 9089:9089 -p 27017:27017 -p 27018:27018 \
-p 27883:27883 -e LICENSE=accept ibmcom/informix-innovator-c:latest
```
*  ```-p```,  expose port ```9088``` to allow remote connections from TCP clients
*  ```-p```,  expose port ```9089``` to allow remote connections from DRDA clients
*  ```-p```,  expose port ```27017``` to allow remote connections from mongo clients
*  ```-p```,  expose port ```27018``` to allow remote connections from REST clients
*  ```-p```,  expose port ```27883``` to allow remote connections from MQTT clients
*  ```--privileged```,  allows Informix Server in Docker Engine to manage kernel configuration
* The default password for user ```informix``` is ```in4mix```.
* By specifying ```-e LICENSE=accept``` parameter, you are accepting this [License](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/4BBCF42D722EB70685257D8F007B6A44?OpenDocument)  to use the software contained in this image.

* The ```docker run``` command will perform a disk initialization for the Informix Database Server.  When you exit this shell the server will be 
taken offline.   

* After disk initialization of the Informix server you should start and stop the server with ```docker start/stop```


2 - Start the Informix Docker container

The docker run command will perform a disk initialization for the Datbase server.  After performing that for the first time you should start the database server with the following command.

```shell
docker start iif_innovator_c
```


3 - Stop the Informix Docker container

The docker run command will perform a disk initialization for the Database server.  After performing that for the first time you should start the database server with the following command.

```shell
docker stop iif_innovator_c
```

4 - To attach to the Informix Docker container (shell)

```shell
docker exec -it iif_innovator_c bash
```

5 - The following command will create a demo database ```stores_demo```:

```shell
$ /opt/IBM/informix/bin/dbaccessdemo
```

6 - Need to know

* Informix is deployed in the Docker Engine in:

    ```shell
      /opt/IBM/informix
    ```

* Informix database server name is  set to ```dev``` by default


### License

View [License Information](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/4BBCF42D722EB70685257D8F007B6A44?OpenDocument)  for the software contained in this image.

### Supported Docker versions

- This image is officially supported on Docker version 1.6.0 or later is required for Docker hosts running Ubuntu or Boot2Docker
- Support for older versions (down to 1.0) is provided on a best-effort basis.

### Community Support
- Zhong Yu (Leo) Wu  -  (leow@ca.ibm.com)
- Darin Tracy -  (darint@us.ibm.com)
- Emerging Technology Team ( imcloud@ca.ibm.com ), IBM Analytics Platform
