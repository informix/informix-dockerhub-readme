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
