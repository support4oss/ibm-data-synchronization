# IBM Data Synchronization

## About IBM Data Synchronization

### What is IBM Data Synchronization?

IBM Data Synchronization is an effortlessly navigable, exceptionally high-functioning data integration solution designed for seamless real-time synchronization of extensive datasets. It excels in the stable and efficient synchronization of tens of billions of records on a daily basis. IBM Data Synchronization combines the power of SeaTunnel with extensive database architecture of IBM Storage Solutions to provide a seamless data integration experience.

* IBM Community Blogpost: https://community.ibm.com/community/user/dataops/blogs/harsh-mittal/2024/01/06/ibm-data-synchronisation-launch-announcement
* User Guide for the IBM Data Synchronization UI: https://community.ibm.com/community/user/dataops/blogs/harsh-mittal/2024/01/08/creating-your-first-ibm-data-synchronization-job

### Why do we need IBM Data Synchronization?

In the realm of modern data management, IBM Data Synchronization proves to be an essential tool. By combining the capabilities of [Apache SeaTunnel OSS](https://seatunnel.apache.org/) with the functionality of IBM UI, it offers reliable performance, a user-friendly design, and real-time synchronization, along with other key features.

For enterprises and users grappling with the complexities of contemporary data management, IBM Data Synchronization emerges as a practical solution. Serving as a one-step solution for Data Integration and Synchronization needs, it provides a straightforward approach to address the challenges associated with managing data in today's landscape. Whether you're dealing with intricate datasets or streamlining data processes, IBM Data Synchronization is a strategic choice for effective and efficient data management.

## How do I download IBM Data Synchronization?

### Step 1

Pull the image from Docker Hub by running the following command:

```
docker pull ibmopensource/ibmseatunnel:datasynchronization-<version>
```

Example:
```
docker pull ibmopensource/ibmseatunnel:datasynchronization-2.3.3.1
```

### Step 2

Check the image is pulled by running the command:

```
docker images
```

### Step 3

Create the following persistent volumes:

* For user data:
```
docker volume create <user_data>
```
where <user_data> is the volume for storing the user data.

* Run the following command to check if the volume is created:
```
docker volume ls
```

### Step 4

Get the Docker container up and running using the following command:

```
docker run -d -v <user_data>:/var/lib/mysql --name ibmdatasynchronization -p 8801:8801 ibmopensource/ibmseatunnel:datasynchronization-<version>
```
where `<user_data>` is the volume created for storing user data

```
NOTE:
If you are using any other way to connect to your database server other than IP address, please use --add-host option to the above docker run command to specify the server name or add the IP address and server name to the /etc/hosts file in a running container. 
```


### Step 5

* Check the container is up and running and other details about the container by using the following command:

```
docker ps -a
```

* Check the container deployment logs using the following command:
```
docker logs ibmdatasynchronization
```

## Deploying a connector plugin using a config file

### From the UI:

```
NOTE: This has been successfully tested only on Mac and Windows operating systems.
```

Access the UI using the following link: `http://<host_ip>:8801`

where `<host_ip>` is the IP address of the host machine. 

The login credentials to access the UI are: 
* `Username: admin`
* `Password: admin`

### From the command line:

#### From the host machine:

Make sure the configuration file exists inside the container, if not, use docker cp or place the config file in the shared mount volume between the host and the container and run the following command:

```
docker exec -it bash -c '$SEATUNNEL_HOME/bin/seatunnel.sh --config /path/to/config/file' ibmdatasynchronization
```

where `$SEATUNNEL_HOME` is `/opt/seatunnel`

#### From inside the container:

* Run the following command to get an interactive bash shell in the container:

```
docker exec -it ibmdatasynchronization bash
```

* Create a config file if it doesn't exist.

* Run the following command to deploy the connector configuration file:

```
$SEATUNNEL_HOME/bin/seatunnel.sh --config /path/to/config/file
```

where `$SEATUNNEL_HOME` is `/opt/seatunnel`

## Restarting the container:

In case a running container stops or exits, perform either of the following steps to restart the container:

### Restarting as the old container:

* Run the following command to get the stopped/exited container ID:

```
docker ps -a
```

* Run the following command to start the stopped container:

```
docker start ibmdatasynchronization
```


* Check if the container is up and running by issuing the following command:

```
docker ps -a
```

### Restarting as a new container:

* Run the following command to deploy a new IBM Data Synchronization container without any data loss from the stopped container:

```
docker run -v <user_data>:/var/lib/mysql --name datasynchronization -d -p 8801:8801 ibmopensource/ibmseatunnel:datasynchronization-<version>
```
where `<user_data>` is the persistent volume for the user data in the stopped container.

* Check if the container is up and running by issuing the following command:

```
docker ps -a
```

## Updating the container:

In case you want to update the container, perform the following steps:

* Stop the container that needs to be updated:

```
docker stop ibmdatasynchronization
```

* Pull the version of the IBM Data Synchronization image that you want to update your container to:

```
docker pull ibmopensource/ibmseatunnel:datasynchronization-<version>
```

* Run the following command to deploy a IBM Data Synchronization container from this new image without any data loss from the stopped container:

```
docker run -d -v <user_data>:/var/lib/mysql --name ibmdatasynchronization -p 8801:8801 ibmopensource/ibmseatunnel:datasynchronization-<version>
```
where `<user_data>` is the persistent volume for the user data in the stopped container.

* Check if the container is up and running by issuing the following command:

```
docker ps -a
```

# What's new in datasynchronization-2.3.6.1

* Docker Architecture Simplification: Combined SeaTunnel Web and SeaTunnel Engine in a single Docker container to streamline deployment, reduce overhead, and offer a unified management and configuration process.

* SQL Support for Task Creation: Introduced support for creating SeaTunnel tasks using SQL alongside the existing HOCON format, offering more flexibility for users in task setup.

* Zeta CDC Sync Enhancements: Improved resource management by releasing idle readers during synchronization and optimizing writing performance with multi-threaded capabilities for better scalability and efficiency.

* Event Notification Mechanism: Added an event notification mechanism for emitting job status changes and DDL updates, allowing for better monitoring and integration with external systems.

* Milvus Vector Database Integration: Introduced support for the vector database Milvus, enhancing SeaTunnel's capabilities in AI and machine learning applications by efficiently handling high-dimensional vectors.

* Other Updates: Added user-defined parameter functions, support for new connectors (Presto, Trino Sink), and improvements to Transform, Zeta Engine, along with various bug fixes and documentation updates.

# Troubleshooting

* If you face any issues, use the following link to register and post the issue: https://community.ibm.com/community/user/dataops/blogs/harsh-mittal/2024/01/08/creating-your-first-ibm-data-synchronization-job