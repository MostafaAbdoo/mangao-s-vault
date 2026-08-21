# Containers
Containers are like Virtual machines but without independent OS; they share the OS(kernel mode) of the physical server
docker uses layers, every change makes a new layer

A **Base Image** is a complete, pre-configured operating system snapshot designed to be copied or referenced to deploy new VMs rapidly.
A **Base Disk** is the actual virtual hard drive file that contains the underlying operating system and core data (common between VMs).
A **Differencing Disk** is a lightweight virtual disk that stores **only the sector-level changes (deltas)** relative to its parent base disk, so the space of the VM would be BS + DS.
-> That's also who containers work, Base kernel + layers
when u finish sitting up the container it *push* to become Image that u can *pull* to be Containers (Like VM Template to Running VMs)
# Docker
It's the software to manage the containers
Containers could be Linux or Windows containers. Containers doesn't save progress unlike VMs
`sudo service docker start` -> run this whenever opening wsl
``` docker
docker image pull $NAME
docker image ls
docker stop $name
docker container ls -a
docker container create -it $IMAGENAME $TERMINALTYPE
terminal type -> like bash
it -> interactive terminal
docker container start -i $NAME
docker container run -it $IMAGENAME    ## create and start and even pull
docker container/image rm $NAME
--name -> to name the container
-h -> to rename the host inside the container
docker image/container inspect $ID
docker exec $name <command>
docker attach $name  -> view the container's live log output in real time and can send keyboard inputs to its main running process.
-p $HostPort : $ContainerPort    -> port mapping to accesss them externaly
-> or u can access it on the same host using internal IP *inspect command to get it*
-v $HostVloumeName : $ContainerVolume  -> volume mapping to store data(volume mounting)
-v $PATH : $ContainerVolume  -> to use an existing volume (bind mounting)
the modern way using this command:
docker run / --mount type=$TYPE, source=$SOURCE, target=$TARGET $IMAGENAME

```
notice when u `exit` it exits the whole container because container = application + kernel = application 
so when u exit an application u exit the container
``` docker
## u could do:
docker container run -it python /bin/bash
bash became PID 1 so when it's terminated the container terminates
so then u write python3 to enter the python env (why u used this image itfp)
so when u write exit rn it will exit python3 env not the whole container cuz u made bash number one process
```
![[Pasted image 20260808080114.png]]
> docker listens on HTTP - 2375
> docker daemon manages objects, CLI uses Docker API to talk to it

Docker uses namespaces to separate processes, for a process to run its ID should be 1, so we use namespaces to make every process(container)'s ID = 1 in it's own environment but they are all running on the same host
we can use CGROUPS to limit and control resources the container uses
# Docker Storage
```
Docker filesystem
/var/lib/docker
   -aufs
   -containers
   -images
   -volumes
```
**Copy-on-Write** is when u want to edit a file that is built in within the image, docker creates a copy of the file so that the changes remain container-based

---
u pull from `hub.docker.com` which is a big registry contains repos for every type
every repo has multiple versions of the same image
image name = `repo : tag`    ex: kali:3.5.2
if u wrote only kali in the command then u r pulling the official image that the docker team recognize
to pull unofficial image : `account name / repo : tag`
to pull the latest one : `repo : latest`

---
An *Image Manifest* is a JSON document that specifies all the pieces (layers) needed to construct a container image for a specific OS/architecture target (e.g., `linux/amd64`).
A *Manifest List* is a higher-level JSON document that groups several individual image manifests under a single image tag (e.g., `redis:latest` or `python:3.11`).

install docker extension on VSCODE
## Build your own image
make a docker file and name it Dockerfile and write the following inside it: (i think this is an example, each app has other requirements and u have to write them instead)
```dockerfile
Dockerfile

FROM Ubuntu -> 1.OS - Ubuntu

RUN apt-get update ->  2.Update apt repo
RUN apt-get install python ->  3.Install dependencies using apt

RUN pip install flask
RUN pip install flask-mysql ->  4. Install Python dependencies using pip

COPY . /opt/source-code   -> 5.Copy source code to /opt folder

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run   -> 6.Run the web server using "flask" command
```
all of the above are layers
```bash
docker build Dockerfile -t nmumshad/my-custom-app ///  docker build -t $NAME .
second one if u on the directory itself
docker push nmumshad/my-custom-app      -> push it to Dokcer Hub
```

---
**environment variable** is a dynamic key-value pair stored by your operating system, Instead of hardcoding values directly inside your source code, you store them in the system's environment as `KEY=value`.
Environment variables are fundamental in containerized apps:

1. **In a `Dockerfile`:**
    ```
    ENV PORT=5000
    ENV APP_ENV=production
    ```
2. **Passing at runtime with `docker run`:**
    ```
    docker run -e PORT=8080 -e DB_HOST=db.local webapp-color:lite
    ```
3. **Using an environment file (`.env`):**
    ```
    docker run --env-file .env webapp-color:lite
    ```
ex) if u want to deploy an application with different backgrounds color u run two images with different colors

---
Command or CMD is the command that the image execute when it runs, u can find and edit it in Dockerfile
ex) default CMD for ubuntu image is bash
u can write it in two formats: CMD command param1 OR CMD ["command" , "param1"]
u can overwrite it by specifying the command after the image name when u run it

Entrypoint is quite similar as if your default CMD was sleep for 10 second but u want to edit in a way u don't need to write the whole command again u can only attach the number of second and the end of the run command and in the Dockerfile u can do this: ENTRYPOINT ["sleep"]

what if i added Entrypoint to Dockerfile but didn't specify a parameter? we need a default value:
ENTRYPOINT["sleep"] , CMD ["5"]

------
## Docker Compose
to setup a complex application running multiple services use Docker Compose
we create `docker-compose.yaml` file and write the different services and the options to run them
then run this command `docker-compose up` to run the whole application
this is an example of how to setup an application the has services dependent on each other:
![[Pasted image 20260813225622.png]]
the link flag is what links them, and the name based on what was written in the code and the name of the service -> `--name`
then write the YAML file and run the command:
![[Pasted image 20260813225859.png]]
instead of `image: voting-app` we can do `build: ./vote` and that location if the location of the folder which have the Dockerfile and the source code the the YAML file will build the image first (i think that mean we don't have to run the command ourselves) and that's available for the images that r not in Docker Hub
![[Pasted image 20260813230628.png]]
in version 2 and up u need to specify the version and u don't need to add links as docker compose create a whole network and add all services to them
but u need to add `depends_on` if a service must go up before another
u can also add environment section in these versions to add environment variables
![[Pasted image 20260813231001.png]]
and that's if u wanna make two separate networks 
# Docker Network
Docker creates 3 networks by default
## Bridge
it is the network the every container attach to by default
## Host
u can attach containers to this network which is your host's network. When assign it to a port it will directly be assigned it to the host's port without the need to map it, u won't be able to assign the same port to another container cuz they all share the same ports
## None 
it's an isolated network with no access to any other network or any other container

`docker network create` -> to create a network
`docker network inspect $NAME`

![[Pasted image 20260818201844.png]]
# Container Orchestration
automated software management of the lifecycle, deployment, scaling, networking, and availability of containerized applications across a cluster of servers.
While running a handful of Docker containers on a single machine can be managed manually, managing hundreds or thousands of microservices across distributed cloud environments requires an automated system to keep applications running smoothly without human intervention.
- Automated Deployment & Scheduling
- Self-Healing & Fault Tolerance
- Auto-Scaling
- Load Balancing & Service Discovery
- Secrets & Config Management
- **Rollouts & Rollbacks:** Manages zero-downtime updates (e.g., rolling updates) by deploying new versions incrementally and reverting automatically if health checks fail.
-> Kubernetes is used to implement Container Orchestration