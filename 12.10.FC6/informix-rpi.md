
![Image of Informix Logo](https://s3.amazonaws.com/ibm.products.us-east-1/informix/informix-logo.jpg)

## What is  IBM Informix Developer Edition ?

```IBM Informix Developer Edition```  is free database software for application development and prototyping.  IBM® Informix® is a secure embeddable database, optimized for OLTP, IoT and is forging new frontiers with its unique ability to seamlessly integrate SQL, NoSQL/JSON, time series and spatial data. Reliability, flexibility and ease of use lets you focus on building applications.


Informix Developer Edition provides the following: 

* __All Informix Enterprise Edition features__ on a variety of platforms.
* __Data access simplified__  via SQL APIs (JDBC,ODBC,.NET), MongoDB APIs, REST API and MQTT API.
* __Web development made easy__  in LAMP, MEAN or other development frameworks. 


>[IBM Informix Family](http://www.ibm.com/analytics/us/en/technology/informix)

>[IBM Informix Developer Edition](http://www-03.ibm.com/software/products/en/infodeveedit)

>[IBM Informix Documentation](https://www.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.welcome.doc/welcome.htm)

## Supported tags

*  ```latest``` 


## **How to use this image** ?
This docker image has to be deployed to Docker Engine on one of supported Cloud providers or your own system. The instructions for creating [Docker Engine](https://docs.docker.com/engine/installation) varies by platform and cloud provider. 

In order to use the image, it is necessary to accept the terms of the Informix Developer Edition license. This is achieved by specifying the environment variable LICENSE equal to accept when running the image.

This docker image contained pre-deployed Informix Developer Edition. By default a sample database (db1) will be created and stored within the container.  

Options are available to use external storage for Informix spaces as well as providing your own __ONCONFIG__ and user __SCHEMA__ file for database creation.


1 - Starting an Informix Docker Container for the First time.

```shell
docker run -it --name ifx --privileged -p 9088:9088 -p 9089:9089 -p 27017:27017 -p 27018:27018 \
-p 27883:27883 -e LICENSE=accept -e DB_INIT=1 ibmcom/informix-rpi:latest
```
*  ```-p```,  expose port ```9088``` to allow remote connections from TCP clients
*  ```-p```,  expose port ```9089``` to allow remote connections from DRDA clients
*  ```-p```,  expose port ```27017``` to allow remote connections from mongo clients
*  ```-p```,  expose port ```27018``` to allow remote connections from REST clients
*  ```-p```,  expose port ```27883``` to allow remote connections from MQTT clients
*  ```--privileged```,  allows Informix Server in Docker Engine to manage kernel configuration
* The default password for user ```informix``` is ```in4mix```, for ```root``` access informix has sudo privileges.
* By specifying ```-e LICENSE=accept``` parameter, you are accepting this [License](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/4BBCF42D722EB70685257D8F007B6A44?OpenDocument)  to use the software contained in this image.

* ```-e DB_INIT=1``` parameter will cause the ````docker```` run command to perform a disk initialization.  When issuing the docker run with the -it option you will be placed in a shell.  When exiting this shell the docker container will be stopped.

* ```-it``` When using this option you will be placed into a shell.  When exiting this shell the docker container will be stopped.

* After disk initialization of the Informix server you should start and stop the server/container with ```docker start/stop```


2 - Start the Informix Docker container

The docker start command will start the container and bring the database online.  It will not perform a disk initialization.  This command is used to start the container after a disk initialization has already occured, and the container is currently not running.

```shell
docker start ifx 
```


3 - Stop the Informix Docker container

The docker stop command  will stop the container and take the database offline.


```shell
docker stop ifx 
```

4 - To attach to the Informix Docker container (shell)

```shell
docker exec -it ifx bash
```

5 - Additional Options __(docker run)__:
* ````-v /home/informix/extvol1:/home/informix/vol1```` This option mounts an external volume __(/home/informix/extvol1)__ to a pre-defined internal volume __(/home/informix/vol1)__   The external volume is a directory created on the host system. 

* This option can be used for external storage of Informix dbspaces and used when a user supplied __ONCONFIG__ and/or __SCHEMA__ file is provided.
* ```-e DB_INIT=1``` This option Will force a Disk Initialization to occur. The docker run command can be issued without this option if an instance was previously created with the docker run command that used this option and external storage was used. 
* ````-e DB_EXTSTORAGE=1```` This option specifies that external storage for Informix spaces should be used.
* ````-e DB_ONCONFIG=onconfig.user```` This option is used to specify a user provided __ONCONFIG__ file.  The __-v__ option must also be used, and the __ONCONFIG__ should be placed in the external storage volume. 
* ````-e DB_SCHEMA=mydb1.sql```` This option is used to specify a schema to create.  The __-v__ option must also be used and the __SCHEMA__ file must be placed in the external storage volume.  The schema file will be run when the __DB_INIT__ option is used.


6 - Example docker run commands: 

* Disk initialization using external storage with user supplied __ONCONFIG__ and __SCHEMA__ 

```shell
docker run -it --name ifx --privileged 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/home/informix/vol1  \
      -e LICENSE=accept                             \
      -e DB_INIT=1                                  \ 
      -e DB_EXTSTORAGE=1                            \
      -e DB_ONCONFIG=onconfig.user                  \ 
      -DB_SCHEMA=iot_db.sql                         \ 
      ibmcom/informix-rpi:latest
```
* Previous Disk initialization using external storage with user supplied __ONCONFIG__ 

```shell
docker run -it --name ifx --privileged 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/home/informix/vol1  \
      -e LICENSE=accept                             \
      -e DB_ONCONFIG=onconfig.user                  \ 
      ibmcom/informix-rpi:latest
```
* Disk initialization using internal storage with user supplied __ONCONFIG__ and __SCHEMA__ 

```shell
docker run -it --name ifx --privileged 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/home/informix/vol1  \
      -e LICENSE=accept                             \
      -e DB_INIT=1                                  \ 
      -e DB_ONCONFIG=onconfig.user                  \ 
      -DB_SCHEMA=iot_db.sql                         \ 
      ibmcom/informix-rpi:latest
```


## License

The Dockerfile and associated scripts are licensed under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0). Informix Developer Edition is licensed under the IBM International License Agreement for Non-Warranted Programs. This license for Informix Developer Edition can be found [online](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/6F8D7862A15D0C628525807B006B92EF?OpenDocument) for the software contained in this image. Note that this license does not permit further distribution.




## Community Support
- [IBM developerWorks](https://developer.ibm.com/answers/search.html?q=informix) 
- [International Informix Users Group](http://members.iiug.org/forums/ids)
- [Stack Overflow](https://developer.ibm.com/answers/search.html?q=informix) 
- [LinkedIn](https://www.linkedin.com/groups/25049)


### Supported Docker versions

- This Informix Docker Image was built using Docker 17.05.0-ce on Raspbian OS (jessie).

### Community Support
- Darin Tracy -  (darint@us.ibm.com)

## Like this image?  Give us a star at the top of this page!  
