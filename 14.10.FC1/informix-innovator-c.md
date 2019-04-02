### What is  IBM Informix Innovator-C ?

```IBM Informix Innovator-C```  eliminates the up-front licensing costs of developing popular database functionality for use in production (redistribution requires a separate license). The Innovator-C Edition is available on Linux, Windows and Mac platforms and is limited to one core and 2 GB of memory.

The Informix Innovator-C Edition on Dockerhub however is not designed for production use.  But it does deliver the following features and benefits:

* __Autonomic features__ minimize system failures and improve performance..
* __Automated backup and restore__ eliminates many manual tasks.
* __Selected support at Elite level__ is available as an optional purchase.

>[IBM Informix Family](http://www-03.ibm.com/software/products/en/informix-family)

>[IBM Informix Innovator-C Edition ](http://www-03.ibm.com/software/products/en/infoinnoedit)

>[IBM Elite Support](http://www-01.ibm.com/support/docview.wss?rs=630&uid=swg21431136)


### Supported tags

*  [latest](http://github.com/informix/informix-dockerhub-readme/blob/master/14.10.FC1/informix-innovator-c.md), 
[14.10.FC1IE](http://github.com/informix/informix-dockerhub-readme/blob/master/14.10.FC1/informix-innovator-c.md),
[12.10.UC12W1IE](http://github.com/informix/informix-dockerhub-readme/blob/master/12.10.FC12/informix-innovator-c.md),
[12.10.UC11IE](http://github.com/informix/informix-dockerhub-readme/blob/master/12.10.FC9/informix-innovator-c.md)


## **How to use this image** ?
This docker image has to be deployed to Docker Engine on one of supported Cloud providers or your own system. The instructions for creating [Docker Engine](https://docs.docker.com/engine/installation) varies by platform and cloud provider. 

In order to use the image, it is necessary to accept the terms of the Informix Innovator-C license. This is achieved by specifying the environment variable LICENSE equal to accept when running the image.

This docker image contains pre-deployed Informix Innovator-C Edition.  The docker images we have stored on __Dockerhub__ are not intended for production purposes and may be have specific functionality removed from the installation directory.

If you have requirements that exceed these you can use the __Github__ [Dockerfiles Repo](https://github.com/informix/informix-dockerifiles) to build your own Docker image.


## Table of Conents
```shell
1 - Running docker container
2 - Starting a docker container
3 - Stopping a docker container 
4 - Attaching to docker container
5 - Storage options
6 - ONCONFIG options
7 - Options
8 - Example docker run commands
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
          -p 27018:27018 -p 27883:27883  informix-db
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
          -p 27018:27018 -p 27883:27883  informix-db
```

### Local Storage 

* You may have a need to store data inside the container.  This is usually a specific use case.  Since containers are ephemeral when you issue a docker run any data you added to the container will be gone.

* ```-e LOCAL=true``` This option will make the container use local storage. 

* There is another use case of __local storage__ and that is to create a container.  Then Modify the container as needed.  Add database/tables, modify chunks/dbspaces.  Then __commit__ that container to a new image and use the newly committed image as your test system.

#### Run with local storage 

```shell
     docker run --name ifx -e LOCAL=true \
          -p 9088:9088 -p 9089:9089 -p 27017:27017        \
          -p 27018:27018 -p 27883:27883  informix-db
```



## 6 - ONCONFIG/Configuration Options: 

### User Supplied ONCONFIG/Configuration

*  ```User supplied files``` require a bind mount point with the -v option so you can place files onto the host before starting/running a docker container.  The rest of the options in this section (User supplied ONCONFIG/Configuration) require the -v option.

* ```User Supplied ONCONFIG``` You can specify an __ONCONFIG__ to be used. Place a file named ```onconfig``` in the external storage volume directory on the host. 

* ```User Supplied sqlhosts``` You can specify an __sqlhosts__ to be used. Place a file named ```sqlhosts``` in the external storage volume directory on the host. Below is a sample sqlhosts file.  Create sqlhosts file with __${HOSTNAME}__ and __${INFORMIXSERVER}__ which are populated during setup, with any other changes you need to the sqlhosts file. 

```shell
############################################################
### DO NOT MODIFY THIS COMMENT SECTION " 
### HOST NAME = ${HOSTNAME} 
############################################################
${INFORMIXSERVER}        onsoctcp        ${HOSTNAME}         9088
${INFORMIXSERVER}_dr     drsoctcp        ${HOSTNAME}         9089
```

* ```sch_init``` When __-e SIZE=custom__ option is used, you can specify a file that will run and perform any configuration, creation of databases, etc. Place a file named ```sch_init_informix.custom.sql``` in the external storage volume directory on the host. 

* ```Update ONCONFIG``` When __-e SIZE=custom__ option is used, you can specify a file that will update the __ONCONFIG__ . Place a file named ```informix_config.custom``` in the external storage volume directory on the host. 


There are 3 sections. 
* [UPDATE] - Updates parameters found in the $ONCONFIG file accordingly
* [DELETE] - Deletes any parameters matching the string
* [ADD] - Adds the specified parameter

Sample __informix_config.custom__
```shell
[UPDATE]
AUTO_TUNE 1
DIRECT_IO 1
DUMPSHMEM 0

[DELETE]
BUFFERPOOL

[ADD]
BUFFERPOOL size=2k,buffers=50000,lrus=8,lru_min_dirty=55,lru_max_dirty=65
```

### Override ONCONFIG params

* ```Override ONCONFIG Parameters``` When __-e ENVFILE=1 --env_file=config.env__ option is used, you can override parameters in the  __ONCONFIG__ .  Create a file with parameters listed out as follows:
You have the ability to ADD, DEL and MOD a parmeter.  If you have parameter that has multiple entries.  To change it, it is best to DEL the parameter first. (Deletes all entries)  Then add the entries you want.

Sample __config.env__
```shell
NETTYPE=ADD:soctcp,4,200,CPU
RESIDENT=MOD:1
BUFFERPOOL=DEL
BUFFERPOOL=ADD:size=2k,buffers=250000,lrus=8,lru_min_dirty=50,lru_max_dirty=60
BUFFERPOOL=ADD:size=4k,buffers=100000,lrus=8,lru_min_dirty=50,lru_max_dirty=60
````


## 7 - Options


### Port Options:

*  ```-p```,  expose port ```9088``` to allow remote connections from TCP clients
*  ```-p```,  expose port ```9089``` to allow remote connections from DRDA clients
*  ```-p```,  expose port ```27017``` to allow remote connections from Mongo clients
*  ```-p```,  expose port ```27018``` to allow remote connections from REST clients
*  ```-p```,  expose port ```27883``` to allow remote connections from MQTT clients
*  ```--privileged```,  allows Informix Server in Docker Engine to manage kernel configuration

### LICENSE option

* By specifying ```-e LICENSE=accept``` parameter, you are accepting this [License](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/4BBCF42D722EB70685257D8F007B6A44?OpenDocument)  to use the software contained in this image.


### Terminal Options

* ```-it``` When using this option you will be placed into a shell and you will see a tail -f of the online.log .  When exiting this shell the docker container will be stopped.

* ```-td``` If you use the -td option instead of ```-it``` option, the container will be started and you will not be placed inside a shell.  So you have to attach to the container to perform basic Informix operations.  This is the __RECOMMENDED__ over the ```-it``` option. 



## 8 - Example docker run commands:

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

The Dockerfile and associated scripts are licensed under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0). Informix Innovator-C Edition is licensed under the IBM International License Agreement for Non-Warranted Programs. This license for Informix Innovator-C Edition can be found [online](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/AFFA2E7CE59C3C72852582CD00635748?OpenDocument) for the software contained in this image. Note that this license does not permit further distribution.



## Community Support
- [IBM developerWorks](https://developer.ibm.com/answers/search.html?q=informix) 
- [International Informix Users Group](http://members.iiug.org/forums/ids)
- [Stack Overflow](https://developer.ibm.com/answers/search.html?q=informix) 
- [LinkedIn](https://www.linkedin.com/groups/25049)



## Like this image?  Give us a star at the top of this page!  
