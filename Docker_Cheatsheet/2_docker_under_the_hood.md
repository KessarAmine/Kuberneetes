# Docker Architecture

Docker simplifies the containerization of applications, while Docker Compose extends this capability to manage multi-container applications, making it easier to define, deploy, and scale complex applications.

Docker is a client-server architecture which means that docker client-deamon are seperate binaries.
Docker client talks with deamons.
deamons handle building running and distributing docker containers.
communication is done with a rest api, UNIX sockets or network interface.
dockerd (docker deamon) mange multiples objects: containers, images, network and volumes...
docker (docker client) sends requests to dockerd
docker registries are images, like docker hub is store for docker images
docker image is a READONLY template of instruction to create a docker container, to create docker images we use Dockerfile that will build an image
docker container is a runnable instance of an image, we can connect containers to networks, attach storage to containers to not lose data, we can new image based on a container, it is isolated
docker services allows us to, scale containers accross multiple deamons, docker swarm, define a desired state.
docker swarm is: multiple docker deamons working together (masters and workers) they communicate through docker API, from docker 1.12 and higher only

# Docker engine

It runs and manages containers, it is modular, you can add/remove swap components like (docker client, Docker deamon, dockerd, runc, containerd)
runc is the implementation of the OCI (open container intiative) container runtime-spec, because the OCI had two spec (runtime spec and image spec), it duty is to create containers
containerd: manage container lifecycle (start, stop, pause, delete), image pull and push

# shim
it is used to decouple running containers from the deamon:
    a contianer is created-> containerd forks an instance of runc for the container-> afet it is done runc exists the process-> shim process is th new parent for the container
this allows us to run many containers without running many runc instances
shim keeps STDIN and STDOUT streams open, so if the deamon restared it is still fine
shim reports the exit status of a container to the docker demon
we make Running Container like so:
    docker container run -it --name <NAME> <IMAGE>:<TAG>
    execution line of this command is like so:
        *.Docker client uses the appropriate API payload
        *.POSTs to the correct API endpoint
        *.Docker deamon receives instructions
        *.Docker deamon calls containerd to start a new container
        *.Docker daemon uses gRPC (a CRUD style API)
        *.containerd creates an OCI bundle from the Docker image
        *.Tells runc to create a container using the OCI bundle
        *.runc interfaces with the OS kernal to get the constructs needed to create a container
        *.This includes namespaces, cgroups, etc.
        *.Container process starts as a child process
        *.runc exits once the container starts
        *.Process is complete, shim is the new parent of the process and container is running

as explained here: https://lucid.app/lucidchart/d7801850-4634-4c63-90c5-2b8d44000032/edit?viewport_loc=517%2C-234%2C2525%2C1334%2C0_0&invitationId=inv_6c7999be-9447-49de-aae2-842e88bf5c58
