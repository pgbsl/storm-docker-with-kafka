storm-docker-with-kafka
=======================

Dockerfiles for building a storm cluster, with kafka also within the cluster. 
This is a fork from wurstmeister/storm-docker, with the addition of kafka (based 100% on [https://github.com/wurstmeister/kafka-docker](https://github.com/wurstmeister/kafka-docker)), the only modification

##Pre-Requisites

- install docker-compose [http://docs.docker.com/compose/install/](http://docs.docker.com/compose/install/)

##Usage

Start a cluster:

- ```docker-compose up```

Destroy a cluster:

- ```docker-compose stop```

Add more supervisors:

- ```docker-compose scale supervisor=3```

##Building

- ```rebuild.sh```

##FAQ
### How can I access Storm UI from my host?
Take a look at docker-compose.yml:

    ui:
      image: wurstmeister/storm-ui:0.9.2
	      ports:
	        - "49080:8080"

This tells Docker to expose the Docker UI container's port 8080 as port 49080 on the host<br/>

If you are running docker natively you can use localhost. 

So, to open storm UI, type the following in your browser:

    localhost:49080

in my case.

### How can I deploy a topology?
Since the nimbus host and port are not default, you need to specify where the nimbus host is, and what is the nimbus port number.<br/>
Following the example above, after discovering the nimbus host IP (could be localhost, could be our docker VM ip as in the case of boot2docker), run the following command:

    storm jar target/your-topology-fat-jar.jar com.your.package.AndTopology topology-name -c nimbus.host=192.168.59.103 -c nimbus.thrift.port=49627

### How can I connect to one of the containers?
Find the forwarded ssh port for the container you wish to connect to (use `docker-compose ps`)

    $ ssh root@`boot2docker ip` -p $CONTAINER_PORT

The password is 'wurstmeister' (from: https://registry.hub.docker.com/u/wurstmeister/base/dockerfile/).

