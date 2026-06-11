#### **container:-**

* It will containerize the services by packing it into a containers so, one services will be independent of

&#x20;  of another service.

* Each services has its respective container and its own File structure.
* Containers are not OS its just miniature of OS
* architecture:-   Hardware->OS->container\_engine->services



### Docker:-

* It is a open source platform which enables to developers to build, deploy, run, update and manage containers.
* Containers are standardized, executable components that combine application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.
* **Docker host:** A Docker host is a physical or virtual machine running Linux (or another Docker-Engine compatible OS).
* **Docker Engine:** Docker engine is a client/server application consisting of the Docker daemon, a Docker API that interacts with the daemon, and a command-line interface (CLI) that talks to the daemon.
* **Docker client:** The Docker client provides the CLI that accesses the Docker API (a REST API) to communicate with the Docker daemon over Unix sockets or a network interface. The client can be connected to a daemon remotely, or a developer can run the daemon and client on the same computer system.
* **Docker objects:** Docker objects are components of a Docker deployment that help package and distribute applications. They include images, containers, networks, volumes, plug-ins and more.
* **Docker containers:** Docker containers are the live, running instances of Docker images. While Docker images are read-only files, containers are live, ephemeral, executable content. Users can interact with them, and administrators can adjust their settings and conditions by using Docker commands.
* **Docker image:** It is a light weight read only file which contains application source code and other dependencies, runtime env, and libraries which is needed for application execution.



###### Docker image vs Docker Container



&#x20;**Feature			Docker Image					Docker Container**

Definition	A read-only template (blueprint) containing code	A runnable, isolated instance of an image.

&#x20;		and dependencies.

Mutability	Immutable: Cannot be changed once created.		Mutable: Has a writable layer for runtime 									changes.

Storage		Stored on disk; often shared via Docker Hub.		Exists in memory (RAM) while running; 										occupies little disk space.

Lifecycle	Created via docker build from a Dockerfile.		Created and started via docker run.

State		Static and inert; does not consume CPU/RAM.		Active and dynamic; consumes system 										resources.

Docker images are like classes and container is like object using docker image we can create multiple containers.



Docker build: Docker build is a command that has tools and features for building Docker images.



Dockerfile: Every Docker container starts with a simple text file containing instructions for how to build the Docker container image. Dockerfile automates the process of creating Docker images. It's essentially a list of CLI instructions that Docker Engine will run to assemble the image.



**Docker Compose:** Developers can use Docker Compose to manage multicontainer applications, where all containers run on the same Docker host. Docker Compose creates a YAML (.YML) file that specifies which services are included in the application and can deploy and run containers with a single command. Because YAML syntax is language-agnostic, YAML files can be used in programs written in Java, Python, Ruby and many other languages.



**Docker Hub:** Its like a GitHub which is public repository that contains docker images



**Docker desktop:** Its a GUI based application where we can see all the docker related information like containers imaged.



##### **VM vs docker:**

* **vm virtualized kernel and application layer and VM can be suitable for any OS**
* **docker virtualizes only application layer and mainly docker uses Linux OS**



#### **Docker Commands:**

* **docker ps**
* **docker ps -a**
* docker run hello-world
* docker run -d -e MYSQL\_ROOT\_PASSWORD=admin --name mysqlolder mysql:8.0
* docker rename 8b8b9bf677fb mysqllatest
* docker stop
* docker rm
* docker run --name mysqllatest \\

&#x20; -p 5000:3306 \\

&#x20; -e MYSQL\_ROOT\_PASSWORD=my-secret-pw \\

&#x20; -d mysql:latest

* docker exec -it mysqlolder //bin/sh



* docker network ls
* docker network create mongo-network



* &#x20; docker run -d \\

&#x20; --name mongo \\

&#x20; -p 27017:27017 \\

&#x20; --network mongo-network \\

&#x20; -e MONGO\_INITDB\_ROOT\_USERNAME=admin \\

&#x20; -e MONGO\_INITDB\_ROOT\_PASSWORD=1234 \\

&#x20; mongo



* &#x20;docker run -d \\

&#x20;-p8081:8081 \\

&#x20;--name mongo-express \\

&#x20;--network mongo-network \\

&#x20;-e ME\_CONFIG\_MONGODB\_ADMINUSERNAME=admin \\

&#x20;-e ME\_CONFIG\_MONGODB\_ADMINPASSWORD=1234 \\

&#x20;-e ME\_CONFIG\_MONGODB\_URL="mongodb://admin:1234@mongo:27017" \\

&#x20; mongo-express



* open localhost:8081 //this is mongo-express default port and local host
* there is standard user name and password //admin pass



###### **Docker compose:**

* It is used to create multicontainer there is a yml/yaml based file which contains some set of scripts which automate the process of building the docker containers.
* docker compose -f docker-compose.yml up
* docker compose -f docker-compose.yml down



###### **App Dockerizing:**

* It is a process of creating the application image to docker containers.
* Dockerfile contains lines of instructions related to app and that leads to build docker image.
* docker build -t node\_js\_app:1.0 .
* docker run -d   --name my-running-app   -p 3001:3001   --network mongo-network   -e MONGO\_URI="mongodb://admin:1234@mongodb:27017/"   node\_js\_app:1.0
* &#x20;docker logs -f my-running-app   //to see output:-- Server running on http://localhost:3001



Feature 		Build Context			Docker Context

Scope		Files used to build an image.	The target Docker engine (local/remote).

Usage		docker build <path>.		docker context use <name>.

Why use it?	To provide source code and 	To manage multiple environments from one terminal.

&#x09;	config to the builder.	



