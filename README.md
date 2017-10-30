# docker-spark

Dockerfiles for ***Apache Spark***.<br>
This is a fork of the Apache Spark image provided by p7hb. 
The original Apache Spark Docker image is available directly from [https://index.docker.io](https://hub.docker.com/u/p7hb/ "» Docker Hub").

This image contains the following softwares:

* OpenJDK 64-Bit v1.8.0_131
* Scala v2.12.2
* SBT v0.13.15
* Apache Spark v2.2.0

### Executing the container as master or worker
The container supports executing as either the spark master or a worker based on either command line arguments or ENV
variables (for use in docker-compose.yml for example). 

From the command line:
	```
	docker container run -h spark-master polydeuce-sys/docker-spark
	```
will launch a spark master.
	```
	docker container run polydeuce-sys/docker-spark worker "spark://spark-master:7777"
	```
will launch a spark worker pointing to as master on the host spark-master with port 7777.
If the second arg is not given, the default spark URL of 
	```
	"spark://spark:7777"
	```
will be used. 

With ENV variables, 
	```
	SPARK_MODE=worker
	SPARK_MASTER=spark://spark-master:7777
	```
would be the equivalent of the example

### Execute Spark job for calculating `Pi` Value

    spark-submit --class org.apache.spark.examples.SparkPi --master spark://spark:7077 $SPARK_HOME/examples/jars/spark-examples*.jar 100
    .......
    .......
    Pi is roughly 3.140495114049511


OR even simpler

    $SPARK_HOME/bin/run-example SparkPi 100
    .......
    .......
    Pi is roughly 3.1413855141385514

Please note the first command above expects Spark Master and Slave to be running. And we can even check the Spark Web UI after executing this command. But with the second command, this is not possible.

### Start Spark Shell

    spark-shell --master spark://spark:7077

### View Spark Master WebUI console

[`http://192.168.99.100:8080/`](http://192.168.99.100:8080/)

### View Spark Worker WebUI console

[`http://192.168.99.100:8081/`](http://192.168.99.100:8081/)

### View Spark WebUI console
Only available for the duration of the application.

[`http://192.168.99.100:4040/`](http://192.168.99.100:4040/)

## Misc Docker commands

### Find IP Address of the Docker machine
This is the IP Address which needs to be used to look upto for all the exposed ports of our Docker container.

    docker-machine ip default

### Find all the running containers

    docker ps

### Find all the running and stopped containers

	docker ps -a

### Show running list of containers

	docker stats --all shows a running list of containers.

### Find IP Address of a specific container

    docker inspect <<Container_Name>> | grep IPAddress

### Open new terminal to a Docker container
We can open new terminal with new instance of container's shell with the following command.

    docker exec -it <<Container_ID>> /bin/bash #by Container ID

OR

    docker exec -it <<Container_Name>> /bin/bash #by Container Name

## License [![License](http://img.shields.io/:license-apache-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.html)
Copyright &copy; 2016 Prashanth Babu.<br>
Licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).
