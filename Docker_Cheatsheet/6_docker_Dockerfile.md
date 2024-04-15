# Dockerfile
 instructions of how to build images, as command, read-only stacked layers
 each layer represents a Dockerfile instruction

# layers
FROM creates a layer from the base image
RUN build the application
CMD specifies what command to run within the container

# guideline
keep containers as ephemeral as possible
follow 6' principle of 12 factor app which is about process: execute the apps as one or more stateless processes
avoid including unnecessary files using .dockerignore
use multi-stage build to reduce size of image, 1 Dockerfile builds 2 images, one of them used to build the artefact inlcude all tools to build and test the image, the second one which will have all the necessary binaries and libraries to run the application, to reduce size
Decpouple applications, one container for each function
Minimize the number of layers, to reduce size
Leverage the build cache

# Insturctions in a Dockerfile
each insturction craetes a layer and each layer created a container as intermidiates steps to get to the final result and then get deleted

From: initializes a new build stage and sets the base image
RUN: Will execute any commands in a new layer, it is close to COPY
CMD: Provides default for an executing container, one CMD in a Dockerfile, this can be done once per Dockerfile
LABEL: Adds metadata to the image, that to add the domains generaly <DOMAINE.ORG.LABEL=VALUE>
EXPOSE: Informs Docker that the container listen to a specified network port at runtime
ENV: sets the env variable
ADD: copies new files, dires or remote files URLs from <SRC> to the filesystem of the image at the path <DEST>
WORKDIR: sets the working dire for any RUN, CMD, ENTRYPOINT, COPY, ADD
ENTRYPOINT: 
ARG: Variables that users can pass at build-time with docker build command using -build-arg <VARNAME>=<VALUE> flag
ONBUILD: A trigger that executes when the image is used as the base for another build
HEALTHCECK: Tells Docker how to test a container to check that it is still working
SHELL: Allows shell to be used and to be overridden

# commands
docker image build -t <name:tag_ofImage> .

# 3' principle of the 12 factir app "CONFIG"
store config in the environment, an app's config is everything that is likely to vary between deploys (staging, production, developer environments, etc.) using env variables, config must be seperated from code part

we can set env varibales in 3 ways:

pass in when building an image:
docker image build .. -env <KEY>=<ALUE>

using an .env file

setting them in Dockerfile:
ENV <KEY>=<VALUE>

use multiple containers one for each stage (dev, or prod) each ysing different PORT

we can change the variable env values in the image build or container creation with the flags --env <VAR_NAME>=<VAR_VALUE>

# Build ARGS

set built-time variables used by Dockerfiles while building the image (dockerfile should be in the same dire to be abel to use . at the end)
cli: --build-arg <NAME>=<VALUE>
Dockerfile: ARG <VAR>=<VALUE>

# Working with non-previleged users
USER instruction creates a non-priveleged user

after building the image with the User, the execution of the container will be using that USER
any insturction after seeting the USER will executed as that USER

to execute the container as root (if that container has non-previleged users) we execute
    docker container exec -u 0 -it <NAME> <cmd_path> (/bin/bash)

the container dire /etc/ is priveleged for superUsers or root

# VOLUME instruction
create a mount point in the container for a dire, give permission to volumes dires..
VOLUME ["...", "..."]

to see the volume name
    docker container inspect <CONTAINER>
look for mounts, type"volume", you can find the destination path, and the name, ....
    docker volume inspect <VOLUME_NAME>
get the mountpoint
    sudo ls -la mountpoint
check if everything is mounted

# ENTRYPOINT
it defines a specefic executable for a container, it cannot be ovveriden without --entrypoint flag

we use ENTRYPOINT ["executable"] if there are flags to add to the executable we add them in CMD["--options"] these will be added to the ARGS of the PATH of the running container in inspect

# dockerignore
most of the ignored files could be:
.md, */.git, /docs/, /tests/
