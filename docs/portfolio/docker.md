---
title: Deploying Servers with Docker
---

> **Context**: The first implementation of Docker in the company.

> **Purpose**: Empower R&D team to start using and testing the new deployment menthod.

> **Audience**: Coworkers with no prior exposure to orchestration tools.

## Docker Terminology

* A Docker **image** is a template for a Docker container. The image contains everything required to run the container, such as dependencies and configurations for a server.
    
	An image is created using a **Dockerfile** which contains instructions (commands). 
	
	For example, you may have a command that says that you are starting from an existing image of the Windows Server operating system, followed by a command that installs a certain Windows update, then a command that installs the .NET Framework, then a command that imports a certain configuration file, and so on.  
	
	Each command in the Dockerfile creates a **layer** in the image. 
	
* A Docker **container** is a runnable instance of an image. You can start or stop a container, just like you would start or stop a server running directly on your machine. 
    
	Running a server image in a container is similar to running it in its own virtual machine. (But not exactly: if you started 10 servers, each in its own VM, you would need the resources to run 10 operating systems. In contrast, you can start 10 containers on the operating system of a single VM.) 

---

## Prerequisites

Before running Docker containers on a machine, you must install the following:

* **Docker**
* **docker-compose**

---

## Compatibility

* Containers can only run if the Windows version of the host machine is newer than the Windows version of the container. Our containers are based on Windows Server 2016 and, therefore, cannot run on older versions of Windows 10.

* Our containers use Hyper-V.

---

## Download Images

To start Docker containers, the Docker images must first be on your hard drive and recognized by the Docker Engine.

You can see the list of available images in the [official repository](https://registry.company.ad/orgs/fake-product/repos).

To get an image, you can either:

* Download the image separately.

* Start a container based on the image. If the image is missing, it will be downloaded automatically.

To download an image, use the `docker pull` command, for example:

`docker pull registry.company.ad/fake-product/cache-server:release`

### Layers

For each image you download, you can see that several steps are run - this is because each step corresponds to a layer in the image. For each layer, the process goes through 3 steps: checking if it already exists, downloading and extracting. After the extraction is done, the pull of the layer is marked as complete.


You can see all the layers that make up an image on the **Layers** tab in the repository.


Most of the layers are common between our images (e.g., the base operating system image). This means the initial pull of the first server image will take a long time, but subsequent image pulls will be much faster as you will already have the common Windows Server image on your hard drive.

### Versions

To store multiple versions of an image, you can tag each version with a different name.

In our internal environment, you can find the following tags for each image:

* `develop` - the latest build
* `release` - the latest released version
* `<build number>` - the respective build

To pull a specific version, run a command such as:

`docker pull registry.company.ad/fake-product/cache-server:7.3.9070`

This command pulls the Cache Server image released for build 9070 of version 7.3.

---

## Run Standalone Containers

After downloading an image, you can start a container based on the image.

!!! note
    In a complex infrastructure like ours, starting containers one by one is not feasible. Containers generally require additional parameters, need to communicate with each other, etc., so they should be run using Docker Compose or Kubernetes.

    However, you can use `docker run` to test container startup and for troubleshooting.

### Start a Container

To start a container, use the `docker run` command:

`docker run <image_identifier>`

You can use a part of the image ID or the full name/tag:

`docker run registry.company.ad/fake-product/cache-server:release`

---

## Run Containers with Docker Compose

Docker Compose is an application that can start several linked services (containers) using a single configuration file.

For each container, you can define various details like what image it should be based on, what other containers it depends on, when to restart, where to store the logs, etc. 

You can then start and stop all defined containers using a single command.

### Generic Docker Compose Deployment

To run a service infrastructure:

1.  Create a `docker-compose.yml` file that defines the services.
2.  Start a PowerShell terminal in the folder where `docker-compose.yml` is saved.
3.  To start the services, run `docker-compose up`.
4.  To stop the services, run `docker-compose down`.

For more information about Docker Compose, see the [official documentation](https://docs.docker.com/compose/).

### Servers Docker Compose Deployment

1.  Get a `docker-compose.yml` file and save it on your hard drive.
2.  Create the folder `C:/FakeProduct` and, inside it, create two subfolders: `config` and `log`.
3.  To configure the base services:
    * In `C:/FakeProduct/config`, copy a `Params.config` file that specifies the database to connect to.
    * The database must be specified in the format `IP:port/server`.
4.  To configure the other servers, you need to determine whether they can start with the sample configuration files (that are already present in the containers). 

    * For each server that needs parameters *not* included in the sample configurations, the `docker-compose.yml` file has a `PROGRAM_CONFIG_URI` entry that points to the configuration file to be used. Configure the file as needed and copy it in `C:/FakeProduct/config`.
	
    * For servers that *can* start with the sample configurations, you don't have to do anything. However, you can add a `PROGRAM_CONFIG_URI` entry to point to a different configuration file if you wish to change some parameters.

	!!! note
		The files on your hard drive are used by the containers because the container are configured so that the `C:/FakeProduct` folder on the host machine is mapped to a folder inside the container. This is done using the `volumes` parameter, explained below.

5.  Start a PowerShell terminal in the folder where `docker-compose.yml` is saved.
6.  Run the command `docker-compose up`.

    The images are downloaded/updated as needed, and then the services start in the order defined in the file.

---

## Docker Compose File Structure

This table explains the structure of the `docker-compose.yml` file from a FakeProduct point of view. For official and generic information, see the [Docker Compose documentation](link-to-docker-compose-docs).

| Docker Compose Parameter | Explanation |
| :--- | :--- |
| `cache-server:` | The name of the service. |
| `image: registry.company.ad/fake-product/cache-server:develop` | The address of the image. When you run `docker-compose up`, Docker checks for the image locally and pulls it if missing. |
| `command: '/p:Params/ServicePorts/DefaultPort=47661'` | Overwrites the value of the `Params > ServicePorts > DefaultPort` parameter. This port is used for WCF services and must be forced for port mapping. |
| `restart: always` | If the server goes down, it will always attempt to restart. |
| `volumes:` | The mappings between folders inside the container and folders on the host machine running Docker Compose. |
| `- C:/FakeProduct:C:/FakeProduct` | The folder `C:/FakeProduct` on the host machine is mapped to the folder `C:/FakeProduct` inside the container. `C:/FakeProduct` must exist on the host machine. |
| `- C:/FakeProduct/log:C:/Servers/log` | The folder `C:/FakeProduct/log` on the host machine is mapped to the folder `C:/Servers/log` inside the container. Server logs will be found in `C:/FakeProduct/log` on the host machine due to this mapping. `C:/FakeProduct/log` must exist on the host machine. |
| `mem_limit: 2g` | The maximum amount of memory the server can use. |
| `depends_on:`<br/> `- 'active-mq'`<br/>`- 'server-registry'`<br/>`- 'status-server'` | Specifies the servers that must start before this server attempts to start. These servers must be defined in `docker-compose.yml` with the same names. If you run `docker-compose up cache-server`, Docker will first start `active-mq`, `service-registry`, and `status-server`, then `cache-server`. Note: `depends_on` only checks if the services are *started*, not if they are *ready*. |
| `environment:` | Environment variables. |
| `- COMMON_PARAMS_URI=C:/FakeProduct/Config/Params.config` | The location of the common configuration file. Due to the `volumes` mapping, creating a `Params.config` file in the `C:/FakeProduct/Config` folder on the host machine will be used by the servers (e.g., to specify the database connection). |
| `- PROGRAM_PARAMS_URI=C:/FakeProduct/Config/CacheServer.config` | The location of the program configuration file. Works the same as the common configuration. |
| `- INSTANCE_NAME=CACHE` | The name of the server instance. |