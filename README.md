# docker-spark

Dockerfile for ***Apache Spark***.<br>
This is a fork of the Apache Spark image provided by p7hb. 
The original Apache Spark Docker image is available directly from [https://index.docker.io](https://hub.docker.com/u/p7hb/ "» Docker Hub").

This image contains the following softwares:

* OpenJDK 64-Bit v1.8.0_131
* Scala v2.12.2
* SBT v0.13.15
* Apache Spark v2.2.0

### Getting the image
You can either build from this repo, or pull from dockerhub using

        docker pull polydeucesys/docker-spark:2.2.0
	
Note the repository name has no hyphen, and that the version tag must be included.

### Executing the container as master or worker
The container supports executing as either the spark master or a worker based on either command line arguments or ENV
variables (for use in docker-compose.yml for example). 

If running master and workers in separate containers, a uer created network should be created to allow host resolution:
        
	docker network create --attachable --driver bridge spark-network
	
From the command line the master can be started as :

	docker container run -p "8080:8080" -p "7077:7077" -p "4040:4040" --network spark-network -h spark --name spark polydeuce-sys/docker-spark
	
	
and the worker as

	
	docker container run  -p "8081:8081"  -h spark-worker --name spark-worker --network spark-network polydeuce-sys/docker-spark worker
	
This will start the worker pointing to the default master URL: 

       spark://spark:7077 
       
If a different spark master URL is required, it can be passed as a second arg when launching the worker.

With ENV variables (please see the example docker-compose config file spark_master_worker.yml), 
	
	SPARK_MODE=worker
	SPARK_MASTER=spark://spark-master:7777
	
would be used to launch a worker pointing to a spark master on spark-master. 

### Testing it is working
Connecting to the spark master webgui on http://hostname:8080 (where hostname is the machine running the master container) should show the master and workers as expected. To further test, once can connect to one of the containers using 

    docker container exec -it <containerid> /bin/bash
    
#### Execute Spark job for calculating `Pi` Value


    spark-submit --class org.apache.spark.examples.SparkPi --master spark://spark-master:7077 $SPARK_HOME/examples/jars/spark-examples*.jar 100
    .......
    .......
    Pi is roughly 3.140495114049511

Please note the first command above expects Spark Master and Slave to be running. And we can even check the Spark Web UI after executing this command. But with the second command, this is not possible.

### Start Spark Shell

    spark-shell --master spark://spark-master:7077


## License [![License](http://img.shields.io/:license-apache-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.html)
Copyright &copy; 2017 Kevin McLellan.<br>
Licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).
