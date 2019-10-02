 

## What is  IBM Informix Developer Sandbox?

```IBM Informix Developer Sandbox```  is a sandbox for application development and prototyping.  


## Supported tags

*  ```latest``` 


## **How to use this image** ?
This docker image has to be deployed to Docker Engine on one of supported Cloud providers or your own system. The instructions for creating [Docker Engine](https://docs.docker.com/engine/installation) varies by platform and cloud provider. 

In order to use the image, it is necessary to accept the terms of the Informix license. This is achieved by specifying the environment variable LICENSE equal to accept when running the image.



1 - Starting the Sandbox Docker Container for the First time.

```shell
docker run -it --name client --privileged -p 9001:9001 -p 27883:27883 -e LICENSE=accept ibmcom/informix-developer-sandbox:latest
```
*  ```-p```,  expose port ```9001``` to allow the use of a Geospatial Demo inside the container. 

* By specifying ```-e LICENSE=accept``` parameter, you are accepting the
License to use the software contained in this image.

* ```-it``` When using this option you will be placed into a shell.  When exiting this shell the docker container will be stopped.

* ```-td``` If you use the -td option instead of ```-it``` option, the container will be started and you will not be placed inside a shell.  So you have to attach to the container. 

2 - Start/Stop the Sandbox Docker container


```shell
docker start/stop ifx 
```

3 - To attach to the Sandbox Docker container (shell)

```shell
docker exec -it client bash
```

## License


Copyright 1996, 2018 IBM Corporation  
Copyright  2019 HCL Technologies 

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.



## Community Support
- [IBM developerWorks](https://developer.ibm.com/answers/search.html?q=informix) 
- [International Informix Users Group](http://members.iiug.org/forums/ids)
- [Stack Overflow](https://developer.ibm.com/answers/search.html?q=informix) 
- [LinkedIn](https://www.linkedin.com/groups/25049)



## Like this image?  Give us a star at the top of this page!  
