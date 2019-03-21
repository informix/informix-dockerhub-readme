 
![Image of Informix Logo]
(https://s3.amazonaws.com/ibm.products.us-east-1/informix/informix-logo.jpg)

## What is  IBM Informix Developer Edition ?

```IBM Informix Developer Edition```  is *free* database software for application development and prototyping.  IBM® Informix® is a secure embeddable database, optimized for OLTP, IoT and is forging new frontiers with its unique ability to seamlessly integrate SQL, NoSQL/JSON, time series and spatial data. Reliability, flexibility and ease of use lets you focus on building applications.


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
docker run -it --name iif_developer_edition --privileged -p 9088:9088 -p 9089:9089 -p 27017:27017 -p 27018:27018 \
-p 27883:27883 -e LICENSE=accept ibmcom/informix-developer-database:latest
```
*  ```-p```,  expose port ```9088``` to allow remote connections from TCP clients
*  ```-p```,  expose port ```9089``` to allow remote connections from DRDA clients
*  ```-p```,  expose port ```27017``` to allow remote connections from mongo clients
*  ```-p```,  expose port ```27018``` to allow remote connections from REST clients
*  ```-p```,  expose port ```27883``` to allow remote connections from MQTT clients
*  ```--privileged```,  allows Informix Server in Docker Engine to manage kernel configuration
* The default password for user ```informix``` is ```in4mix```, for ```root``` access informix has sudo privileges.
* By specifying ```-e LICENSE=accept``` parameter, you are accepting this [License](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/4BBCF42D722EB70685257D8F007B6A44?OpenDocument)  to use the software contained in this image.


* ```-it``` When using this option you will be placed into a shell.  When exiting this shell the docker container will be stopped.

* ```-td``` If you use the -td option instead of ```-it``` option, the container will be started and you will not be placed inside a shell.  So you have to attach to the container to perform basic Informix operations.

* The ```docker run``` command will perform a disk initialization for the Informix Database Server.  When you exit this shell the server will be taken offline.   

* After disk initialization of the Informix server you should start and stop the server with ```docker start/stop```


2 - Start the Informix Docker container

The docker start command will start the container and bring the database online.  It will not perform a disk initialization.  This command is used to start the container after a disk initialization has already occured, and the container is currently not running.

```shell
docker start iif_developer_edition
```


3 - Stop the Informix Docker container

The docker stop command  will stop the container and take the database offline.


```shell
docker stop iif_developer_edition
```

4 - To attach to the Informix Docker container (shell)

```shell
docker exec -it iif_developer_edition bash
```

5 - Additional Options __(docker run)__:
* ```-e DB_EXTSTORAGE=1``` This option specifies that external storage for Informix dbspaces should be used.  If this option is used you need to use the -v option to mount external storage.

* ```-v /home/informix/extvol1:/home/informix/vol1``` This option mounts an external volume __(/home/informix/extvol1)__ to a pre-defined internal volume __(/home/informix/vol1)__   The external volume is a directory created on the host system. 

* ```-e DB_ONCONFIG=onconfig.user``` This option is used to specify a user provided __ONCONFIG__ file.  The __-v__ option must also be used, and the __ONCONFIG__ should be placed in the external storage volume. 

* ```-e DB_INIT=1``` This option Will force a Disk Initialization to occur. If the docker run command is issued using internal storage (__DB_EXTSTORAGE__ is not used) then this option is not necessary.  A Disk initialization will happen every time __docker run__ is executed.  <br><br>
If using External storage (__DB_EXTSTORAGE__) you can issue __docker run__ without the __DB_INIT__ option and disk initialization will not occur.

* ```-e DB_SCHEMA=mydb1.sql``` This option is used to specify a schema (sql) file to run on initialization.  The __-v__ option must also be used and the __SCHEMA__ file must be placed in the external storage volume.  The schema file will be run when the __DB_INIT__ option is used.  If this option is not used a default schema file is run (iot_db.sql).

* ```-e DB_SBSPACE=1``` This option is used when you want to perform a disk initialization and you need an sbspace created. 


6 - Example docker run commands: 

* Default run command with no additional options.  This method stores the Informix dbspaces inside the container and will initialize the disk.

```shell
docker run -it --name ifx --privileged 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/home/informix/vol1  \
      -e LICENSE=accept                             \
      ibmcom/informix-developer-database:latest
```

* Disk initialization using internal storage with user supplied __ONCONFIG__ and __SCHEMA__ .  Note that the -e DB_INIT=1 is not needed since this method is using internal storage.  IE, every docker run using internal storage will initialize disk.

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
      -e DB_SCHEMA=mydb.sql                         \ 
      ibmcom/informix-developer-database:latest
```

* This example will initialize disk storing the dbspaces in the external storage and use a default __ONCONFIG__ and __SCHEMA__ files.

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
      ibmcom/informix-developer-database:latest
```

* This example will initialize disk storing the dbspaces in the external storage and use a user supplied __ONCONFIG__ and __SCHEMA__ 

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
      -e DB_SCHEMA=mydb.sql                         \ 
      ibmcom/informix-developer-database:latest
```

* This example will not perform disk initialization but you have to have used storage. 

```shell
docker run -it --name ifx --privileged 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/home/informix/vol1  \
      -e LICENSE=accept                             \
      -e DB_EXTSTORAGE=1                            \ 
      ibmcom/informix-developer-database:latest
```

## License

The Dockerfile and associated scripts are licensed under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0). Informix Developer Edition is licensed under the IBM International License Agreement for Non-Warranted Programs. This license for Informix Developer Edition can be found [online](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/6F8D7862A15D0C628525807B006B92EF?OpenDocument) for the software contained in this image. Note that this license does not permit further distribution.




## Community Support
- [IBM developerWorks](https://developer.ibm.com/answers/search.html?q=informix) 
- [International Informix Users Group](http://members.iiug.org/forums/ids)
- [Stack Overflow](https://developer.ibm.com/answers/search.html?q=informix) 
- [LinkedIn](https://www.linkedin.com/groups/25049)



## Like this image?  Give us a star at the top of this page!  
