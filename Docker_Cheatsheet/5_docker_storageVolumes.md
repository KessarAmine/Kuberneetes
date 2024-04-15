# docker storage and volume
there are categories of data storage
1. non persistent data:
    data that is ephemeral like application code
    every contianer has in r/w layer
    local storage, snapshot storage
2. persistant data:
    volumes, they are decoupled from containers
    create the volume, create the container, mount the volume to a diretory in the container
    deleting the container wont delete the volume
    first-class
    uses the local driver
    third-party drivers: block storage, file storage, object storage
    /var/lib/docker/vlolumes

# docker commands

docker volume -h
docker volume ls
docker volume create <NAME>
docker volume inspect <NAME>
docker volume rm <NAME>
docker volume prune

# using bind mounts
Bind mounts have been around since the early days of Docker. They have limited functionality compared to volumes. With bind mount, a file or directory on the host machine is mounted into a container.
can be craeted using mount flag or volume flag
usfull in config files for example, whem we want to mount one file....

1. Using the mount flag:
    mkdir target
    docker container run -d \
    --name <NAME \
    --mount type=bind,source="$(pwd)"/<SORUCE>,target=<TARGET> \
    <IMAGE>

notes: 
bind mounts are not managed by volume subcommands
to check if the mount is correct, run docker container inspect <NAME>, check mounts parent...
any changes on the source (source="$(pwd)"/targe) file in the host will be bined inside the container (target=/app) and visversa

2. Using the volume flag:
* we dont need to create target2 dir
    docker container run -d \
    --name <NAME>\
    -v "$(pwd)"/<SOURCE>:<TARGET> \
    <NAME>

# using volumes
they are easier to backup and migrate, can be shared through containers
can be created using mount flag or volume flag

docker volume create html-volume

docker container run -d \
 --name nginx-volume1 \
 --mount type=volume,source=html-volume,target=/usr/share/nginx/html/ \
 nginx

docker container run -d \
 --name nginx-volume2 \
 -v html-volume:/usr/share/nginx/html/ \
 nginx

# Using a readonly volume:

docker run -d \
  --name=nginx-volume3 \
  --mount source=html-volume,target=/usr/share/nginx/html,readonly \
  nginx

