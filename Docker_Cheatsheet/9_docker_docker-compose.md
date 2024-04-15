# docker-compose
manage mulitple microservices
# docker compose commands
shoudl bein the same dire or it would be
docker-compose -f <PATHTOFILE> [cmd]
docker-compose
    up: create and restart containers
    ps: list containers
    stop: stop services
    start: start services
    restart: restart services
    down: stop and remove containers, networks, images and volumes

create a compose service in the bg:
docker-compose up -d
# docker-compose.yml or json as alternative

version: compose file format and tied to the docker engine
serices: each service is a seperate container that is going to be built part of the app
build: will use the Dockerfile to build the container
context: scope of the file
Dockerfile: sepcify the name of the dockerfile
args: to pass as params if the dockerfile has args
ports: maps ports as we normally do refering to the dockerfile, it can be an array
environment: this allows to set env_vars to the docker container
container_name: lol container name
image: if we do not need a dockerfile to build the image we do not need to specify tge build with cintext and dockerfile and args to the compose file and just use image option "<IMAGE>:<TAG>"
volumes: list of volumes to be mounted on the container, we can use the short syntax, nameofvalue:mountpoint or we can use the large syntax using: type, source, target and volume
networks: networks that the container will be attached to
depends_on: service dependency between 1 or more services 

volumes: nameofthevolume:   driver_opts: define our driver option with type, o, device
