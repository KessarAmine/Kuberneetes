docker -h 
we have base cammand that are like docker image, docker container, docker swarm....
we can inspect an image or a container by requesting docker container insoect <container_ID/IMAGE_ID>
we have a good command that is docker container top, it gives us the pid of the process that the container is running ()master process and worker process
we can use curl to test if the port of the container is fine after the container is running
we can see what happened in a container by doing: docker container logs <container_ID>
docker container stats <container_ID> to see how much is the container is using ressources
docker container run: makes a container run
docker container exec: allows us to execute command inside a running container using a flag -it (interactive tt1) then the cmd
si like this we can go insinde the container and go though it with exectuing commands
docker container pause <container_ID>: it pauses all the running processes inside the running container
docker container prune -f: remove all stopped containers, -f to force the prompt into a yes
