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
docker pull ghcr.io/support4oss/ibmseatunnel:datasynchronization-<version>
```

Example:
```
docker pull ghcr.io/support4oss/ibmseatunnel:datasynchronization-2.3.6.1
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
docker run -d  -e MYSQL_ROOT_PASSWORD=your_password  -e "TZ=Asia/Kolkata" -v <user_data>:/var/lib/mysql --name ibmdatasynchronization -p 8801:8801 ghcr.io/support4oss/ibmseatunnel:datasynchronization-<version>
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
* `Password: IBM@DATA#SYNC@2024`

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
docker run -d  -e MYSQL_ROOT_PASSWORD=your_password  -e "TZ=Asia/Kolkata" -v <user_data>:/var/lib/mysql --name ibmdatasynchronization -p 8801:8801 ghcr.io/support4oss/ibmseatunnel:datasynchronization-<version>
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
docker pull ghcr.io/support4oss/ibmseatunnel:datasynchronization-<version>
```

* Run the following command to deploy a IBM Data Synchronization container from this new image without any data loss from the stopped container:

```
docker run -d  -e MYSQL_ROOT_PASSWORD=your_password  -e "TZ=Asia/Kolkata" -v <user_data>:/var/lib/mysql --name ibmdatasynchronization -p 8801:8801 ghcr.io/support4oss/ibmseatunnel:datasynchronization-<version>
```
where `<user_data>` is the persistent volume for the user data in the stopped container.

* Check if the container is up and running by issuing the following command:

```
docker ps -a
```

# What's new in datasynchronization-2.3.6.1

* **SQL-Based Task Creation**: Support for creating IBM Data Synchronization tasks using SQL, in addition to the existing HOCON format.
* **Kafka as Virtual Table**: Kafka topics can be managed as virtual tables, enabling real-time data processing and flexible transformations within the IBM Data Synchronization interface.
* **Full SQL Transformation Support**: Improved support for SQL transformations to address integration job issues, with a structured graphical interface for managing data pipelines.
* **WatsonX.data Integration**: Enhanced ETL capabilities and real-time data transformation to integrate with WatsonX.data for streamlined data management and analytics.
* **Hazelcast Cluster and Database Connectivity Dashboard**: New dashboard to monitor Hazelcast cluster health and database connectivity, tracking key metrics like active nodes, connection counts, and system load.
* **User-Defined Parameter Functions and New Connectors**: Added support for user-defined parameter functions and multiple connectors (e.g., Presto, Db2, Trino Sink), along with updates to SQL transform functionality and Zeta Engine.

# Troubleshooting

* If you face any issues, use the following link to register and post the issue: https://community.ibm.com/community/user/dataops/blogs/harsh-mittal/2024/01/08/creating-your-first-ibm-data-synchronization-job
