
# What is  IBM Informix Developer Edition ?

```IBM Informix Developer Edition```  is free database software for application development and prototyping.  IBM® Informix® is a secure embeddable database, optimized for OLTP, IoT and is forging new frontiers with its unique ability to seamlessly integrate SQL, NoSQL/JSON, time series and spatial data. Reliability, flexibility and ease of use lets you focus on building applications.


Informix Developer Edition provides the following: 

* __All Informix Enterprise Edition features__ on a variety of platforms.
* __Data access simplified__  via SQL APIs (JDBC,ODBC,.NET), MongoDB APIs, REST API and MQTT API.
* __Web development made easy__  in LAMP, MEAN or other development frameworks. 


>[IBM Informix Family](http://www.ibm.com/analytics/us/en/technology/informix)

>[IBM Informix Developer Edition](http://www-03.ibm.com/software/products/en/infodeveedit)

>[IBM Informix Documentation](https://www.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.welcome.doc/welcome.htm)

## Supported tags & Documentation

*  [latest](http://github.com/informix/informix-dockerhub-readme/blob/master/14.10.FC1/informix-developer-database.md), 
[14.10.FC1DE](http://github.com/informix/informix-dockerhub-readme/blob/master/14.10.FC1/informix-developer-database.md),
[12.10.UC12W1DE](http://github.com/informix/informix-dockerhub-readme/blob/master/12.10.FC12/informix-developer-database.md),
[12.10.UC11DE](http://github.com/informix/informix-dockerhub-readme/blob/master/12.10.FC9/informix-developer-database.md)



### LICENSE option

* By specifying ```-e LICENSE=accept``` parameter, you are accepting this [License](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/4BBCF42D722EB70685257D8F007B6A44?OpenDocument)  to use the software contained in this image.


### Terminal Options

* ```-it``` When using this option you will be placed into a shell and you will see a tail -f of the online.log .  When exiting this shell the docker container will be stopped.

* ```-td``` If you use the -td option instead of ```-it``` option, the container will be started and you will not be placed inside a shell.  So you have to attach to the container to perform basic Informix operations.  This is the __RECOMMENDED__ over the ```-it``` option. 


## 8 - Create Self contained Test system: 

* If you have a need to create a self contained image.  An image that will store your database(s)/table(s) inside the container.  An image that you can modify the dbspaces/chunks and store all this inside the container.

* This is essentially a test system with data already placed inside it.  This can be done easily with the following steps:

### OPTION 1
### 1.  Start an image using the -e STORAGE=local

```shell
      docker run --name ifx -h ifx -e STORAGE=local            \
            -p 9088:9088 -p 9089:9089 -p 27017:27017    \
            -p 27018:27018 -p 27883:27883 ibmcom/informix-developer-database:latest 
```

### 2.  Create databases/tables.  Modify chunks/dbspaces, etc.

### 3.  Bring the Informix instance offline.  Within the Container run:

```shell
      onmode -ky
```

### 4.  Commit the container to a new image.  On the host run:

```shell
            docker commit ifx ifx-test:v1 
```

* Now you have a new __Docker image__ named ifx-test:v1 that you can run that will contain your databases/tables, chunk/dbspace layout all stored inside the container.  To run this you would run the following:

```shell
      docker run --name ifx -h ifx                   \
            -p 9088:9088 -p 9089:9089 -p 27017:27017 \
            -p 27018:27018 -p 27883:27883 ifx-test:v1
```


### OPTION 2

* Option #1 will configure the system for you and use local storage.  If you want full control to setup the system you can do the following:

### 1.  Start an image using the -e CONFIGURE_INIT=no

```shell
      docker run --name ifx -h ifx 
            -e CONFIGURE_INIT=no            \
            ibmcom/informix-developer-database:latest 
```

### 2.  Start an image using the -e CONFIGURE_INIT=no

* This file controls the startup of the container.  Modify this accordingly.  This file will start the Informix server and the Wire Listener.

### 3.  Follow the rest of the steps from OPTION 1 for a Custom System

## 9 - Example docker run commands:

* Default run command with no additional options.  This method stores the Informix dbspaces in an unnamed volume.

```shell
docker run -it --name ifx -h ifx  
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -e LICENSE=accept                             \
      ibmcom/informix-developer-database:latest
```

* Run command that uses external (host) directory __(-v /home/informix/extvol:opt/ibm/data)__ for volume storage, and configures the system for oltp __(-e TYPE=oltp)__.

```shell
docker run -it --name ifx -h ifx  
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/opt/ibm/data        \
      -e TYPE=oltp                                  \
      -e LICENSE=accept                             \
      ibmcom/informix-developer-database:latest
```

* Run command that uses external (host) directory for volume storage, and configures the system for oltp.  This command limits the container to 4 cpus __(--cpus="4")__ and the memory to 4gb __(--memory="4000m")__.  

```shell
docker run -it --name ifx -h ifx  
      --cpus="4" --memory="4000m"                   \ 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/opt/ibm/data        \
      -e TYPE=oltp                                  \
      -e LICENSE=accept                             \
      ibmcom/informix-developer-database:latest
```


## License

The Dockerfile and associated scripts are licensed under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0). Informix Developer Edition is licensed under the IBM International License Agreement for Non-Warranted Programs. This license for Informix Developer Edition can be found [online](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/DB4F392B782CB8C7852582CD00635BB9?OpenDocument) for the software contained in this image. Note that this license does not permit further distribution.


## Community Support
- [IBM developerWorks](https://developer.ibm.com/answers/search.html?q=informix) 
- [International Informix Users Group](http://members.iiug.org/forums/ids)
- [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=informix)
- [IBM Informix Community](https://community.ibm.com/community/user/hybriddatamanagement/communities/community-home?communitykey=cf5a1f39-c21f-4bc4-9ec2-7ca108f0a365&tab=groupdetails)



## Like this image?  Give us a star at the top of this page!  
