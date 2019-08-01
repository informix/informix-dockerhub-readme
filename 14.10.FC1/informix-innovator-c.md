# What is  IBM Informix Innovator-C ?

```IBM Informix Innovator-C```  eliminates the up-front licensing costs of developing popular database functionality for use in production (redistribution requires a separate license). The Innovator-C Edition is available on Linux, Windows and Mac platforms and is limited to one core and 2 GB of memory.

The Informix Innovator-C Edition on Dockerhub however is not designed for production use.  But it does deliver the following features and benefits:

* __Autonomic features__ minimize system failures and improve performance..
* __Automated backup and restore__ eliminates many manual tasks.
* __Selected support at Elite level__ is available as an optional purchase.

>[IBM Informix Family](http://www-03.ibm.com/software/products/en/informix-family)

>[IBM Informix Innovator-C Edition ](http://www-03.ibm.com/software/products/en/infoinnoedit)

>[IBM Elite Support](http://www-01.ibm.com/support/docview.wss?rs=630&uid=swg21431136)


## Supported tags & Documentation

*  [latest](http://github.com/informix/informix-dockerhub-readme/blob/master/14.10.FC1/informix-innovator-c.md), 
[14.10.FC1IE](http://github.com/informix/informix-dockerhub-readme/blob/master/14.10.FC1/informix-innovator-c.md),
[12.10.UC12W1IE](http://github.com/informix/informix-dockerhub-readme/blob/master/12.10.FC12/informix-innovator-c.md),
[12.10.UC11IE](http://github.com/informix/informix-dockerhub-readme/blob/master/12.10.FC9/informix-innovator-c.md)


## **How to use this image** ?
This docker image has to be deployed to Docker Engine on one of supported Cloud providers or your own system. The instructions for creating [Docker Engine](https://docs.docker.com/engine/installation) varies by platform and cloud provider. 

In order to use the image, it is necessary to accept the terms of the Informix Innovator-C license. This is achieved by specifying the environment variable LICENSE equal to accept when running the image.

This docker image contains pre-deployed Informix Innovator-C Edition.  The docker images we have stored on __Dockerhub__ are not intended for production purposes and may be have specific functionality removed from the installation directory.

If you have requirements that exceed these you can use the __Github__ [Dockerfiles Repo](https://github.com/informix/informix-server-dockerfiles) to build your own Docker image.


## Table of Conents
```shell
1 - Running docker container
2 - Starting a docker container
3 - Stopping a docker container 
4 - Attaching to docker container
5 - Storage options
6 - ONCONFIG options
7 - Options
8 - Create self contained Test system
9 - Example docker run commands
```

## 1 - Starting an Informix Docker Container for the First time.

```shell
docker run -it --name ifx --privileged -p 9088:9088 -p 9089:9089 -p 27017:27017 -p 27018:27018 \
-p 27883:27883 -e LICENSE=accept ibmcom/informix-innovator-c:latest
```

* The ```docker run``` command will perform a disk initialization for the Informix Database Server.  When you exit this shell the server will be taken offline.   

* After disk initialization of the Informix server you should start and stop the server with ```docker start/stop```


## 2 - Start the Informix Docker container

The docker start command will start a (stopped) container and bring the database online.  It will not perform a disk initialization.  This example starts a container named ifx.  Initially run with ```--name ifx```.  

```shell
docker start ifx 
```


## 3 - Stop the Informix Docker container

The docker stop command  will stop a running container and take the database offline. This example stops a container named ifx.  Initially run with ```--name ifx```.


```shell
docker stop ifx 
```

## 4 - To attach to the Informix Docker container (shell) that was initially run with the ```--name ifx```

```shell
docker exec -it ifx bash
```

## 5 - Storage Options:

The docker image supports __anonymous volumes__, __named volumes__ and __bind mounts__.  

### Anonymous Volume

*  Default behavior is __anonymous volume__.  When you issue the docker run command it will create an anonymous volume which can be seen in a ```docker volume ls``` command. 
* When the ```-v``` option is NOT used on the docker run command an anonymous volume will be used.

### Named Volume

* __Named volumes__ can be used, use the ```-v``` option and follow these instructions:

* ```-v ifx-vol:/opt/ibm/data``` This option mounts a named volume __(ifx-vol)__ to a pre-defined internal volume __(/opt/ibm/data)__   For more information on the named volume see the ```docker volume``` command. 

#### Create a named  volume:
```shell
   docker volume create ifx-vol
```
#### Run with a named volume
```shell
     docker run --name ifx -v ifx-vol:/opt/ibm/data \
          -p 9088:9088 -p 9089:9089 -p 27017:27017  \
          -p 27018:27018 -p 27883:27883 ibmcom/informix-innovator-c:latest 
```

### Bind Mount

* __Bind mounts__ can be used, use the -v option and follow these instructions:


#### Create mount point on Host 

```shell
   Example:  mkdir /home/informix/extvol
```

#### Run with bind mount 

```shell
     docker run --name ifx -v /home/informix/extvol:/opt/ibm/data \
          -p 9088:9088 -p 9089:9089 -p 27017:27017        \
          -p 27018:27018 -p 27883:27883 ibmcom/informix-innovator-c:latest 
```

### Local Storage 

* You may have a need to store data inside the container.  This is usually a specific use case.  Since containers are ephemeral when you issue a docker run any data you added to the container will be gone.

* ```-e STORAGE=local``` This option will make the container use local storage. 

* A use case of __local storage__ is to create a container then modify the container as needed.  Add database/tables, modify chunks/dbspaces.  Then __commit__ that container to a new image and use the newly committed image as your test system.

#### Run with local storage 

```shell
     docker run --name ifx -h ifx -e STORAGE=local\
          -p 9088:9088 -p 9089:9089 -p 27017:27017        \
          -p 27018:27018 -p 27883:27883 ibmcom/informix-innovator-c:latest 
```



## 6 - User supplied Configuration Options: 

To use  ```user supplied configuration files``` you must use a bind mount and mount the __/opt/ibm/config__ volume then you specify each file that will be user supplied. This volume is used specifically for configuration files and will not be used for storage of informix dbspaces and chunks.  On the ```docker run``` command you would add the following:

``` 
      -v /home1/setupvol:/opt/ibm/config 
```

### User Supplied Files 


* ```User Supplied ONCONFIG``` You can specify an __ONCONFIG__ file to be used by placing the file in hosts directory that is mounted to the __config volume__.  In the example above it would be __/home1/setupvol__.  Use the -e option to specify the file name.

```
      -e ONCONFIG_FILE=onconfig 
``` 

* ```User Supplied SQLHOSTS ``` You can specify an __sqlhosts__ file to be used by placing the file in hosts directory that is mounted to the __config volume__.  In the example above it would be __/home1/setupvol__.  Use the -e option to specify the file name.

```
      -e SQLHOSTS_FILE=sqlhosts
``` 

* ```User Supplied sch_init_xxxxx.sql``` You can specify an __sch_init_xxxxx.sql__ file to be used by placing the file in hosts directory that is mounted to the __config volume__.  In the example above it would be __/home1/setupvol__.  This file is used during startup to perform additional initialization options.  ex.  creation of dbspaces, databases, etc.  Use the -e option to specify the file name.

```
      -e INIT_FILE=my_init.sql
``` 

* ```User Supplied Shell scripts``` You can specify an __PRE_INIT__ and __POST_INIT__ shell script to be executed.  These execute prior to the Informix server being disk initialized and after the disk initialization.  Place these files in the hosts directory that is mounted to the __config volume__.  In the example above it would be __/home1/setupvol__.  Make sure the shell scripts have execute permission on them.  

```
      -e RUN_FILE_PRE_INIT=my_pre.sh
      -e RUN_FILE_POST_INIT=my_post.sh
``` 

* ```User Supplied Shell script``` To control __Disk initialization__.  This option allows you to specify a shell script to be used to do all configuration.  The docker container will not perform disk initialization or setup. Place this file in the hosts directory that is mounted to the __config volume__.  In the example above it would be __/home1/setupvol__.  Make sure the shell script have execute permission on them.  

```
      -e CONFIGURE_INIT=my_pre.sh
``` 



## 7 - Options


### Port Options:

*  ```-p```,  expose port ```9088``` to allow remote connections from TCP clients
*  ```-p```,  expose port ```9089``` to allow remote connections from DRDA clients
*  ```-p```,  expose port ```27017``` to allow remote connections from Mongo clients
*  ```-p```,  expose port ```27018``` to allow remote connections from REST clients
*  ```-p```,  expose port ```27883``` to allow remote connections from MQTT clients
*  ```--privileged```,  allows Informix Server in Docker Engine to manage kernel configuration

### Sizing Options:

* ```-e TYPE=[oltp|dss|hybrid]``` will configure your Informix server accordingly.  Your informix server will be configured to use all available resources given to the container.  So to limit the amount of cpu and memory used for a given container it is specified on the docker run command.  Type of oltp will use more memory for your bufferpool.  Type of dss will use more memory for shmvirt and a type of hybrid will be be 50/50.

* ```-e SIZE=[small|medium|large]``` will configure your Informix server based on size. This will impact the shared memory used as well dbspace creation.

### INITIALIZATION:

* ```-e CONFIGURE_INIT=no``` This is a variable that has two purposes.  As mentioned previously it is used bypass all setup/ininitialization and  run a shell script to perform your own setup.  If you merely want to skip setup/initialization you can set this parameter to no.  Then attach to the container and configure as needed.


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
            -p 27018:27018 -p 27883:27883 ibmcom/informix-innovator-c:latest 
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
            ibmcom/informix-innovator-c:latest 
```

### 2.  Modify /opt/ibm/scripts/informix_entry.sh 

* This file controls the startup of the container.  Modify this accordingly.  This file will start the Informix server and the Wire Listener.

### 3.  Follow the rest of the steps from OPTION 1 for a Custom System 

### 4.  Notes to understand when creating a self contained system.

* There is a default environment script at /usr/local/bin/informix_inf.env.  
* For Data storage it is important to understand the /opt/ibm/data is a mounted volume.  That could be bind mount, anonymous volume or named volume.
* If you want to store data inside the container, make a directory /opt/ibm/localdata and store files and chunks here.


## 9 - Example docker run commands:

* Default run command with no additional options.  This method stores the Informix dbspaces in an unnamed volume.

```shell
docker run -it --name ifx --privileged 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -e LICENSE=accept                             \
      ibmcom/informix-innovator-c:latest
```

* Run command that uses external (host) directory __(-v /home/informix/extvol:opt/ibm/data)__ for volume storage, and configures the system for oltp __(-e TYPE=oltp)__.

```shell
docker run -it --name ifx --privileged 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/opt/ibm/data        \
      -e TYPE=oltp                                  \
      -e LICENSE=accept                             \
      ibmcom/informix-innovator-c:latest
```

* Run command that uses external (host) directory for volume storage, and configures the system for oltp.  This command limits the container to 4 cpus __(--cpus="4")__ and the memory to 4gb __(--memory="4000m")__.  

```shell
docker run -it --name ifx --privileged 
      --cpus="4" --memory="4000m"                   \ 
      -p 9088:9088                                  \
      -p 9089:9089                                  \
      -p 27017:27017                                \ 
      -p 27018:27018                                \ 
      -p 27883:27883                                \ 
      -v /home/informix/extvol:/opt/ibm/data        \
      -e TYPE=oltp                                  \
      -e LICENSE=accept                             \
      ibmcom/informix-innovator-c:latest
```



## License

The [Dockerfiles](https://github.com/informix/informix-server-dockerfiles) are associated scripts and licensed under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0). Informix Innovator-C Edition is licensed under the IBM International License Agreement for Non-Warranted Programs. This license for Informix Innovator-C Edition can be found [online](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/AFFA2E7CE59C3C72852582CD00635748?OpenDocument) for the software contained in this image. Note that this license does not permit further distribution.



## Community Support
- [IBM developerWorks](https://developer.ibm.com/answers/search.html?q=informix) 
- [International Informix Users Group](http://members.iiug.org/forums/ids)
- [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=informix)
- [IBM Informix Community](https://community.ibm.com/community/user/hybriddatamanagement/communities/community-home?communitykey=cf5a1f39-c21f-4bc4-9ec2-7ca108f0a365&tab=groupdetails)



## Like this image?  Give us a star at the top of this page!  
