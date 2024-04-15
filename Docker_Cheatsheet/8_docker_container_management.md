# inspecting running process in our containers

we can display all runnign process of a container:
docker container top <NAME>
or exec the contianer with -it and run top inside of the container

we can display a live stream of container resources usage
docker container stats <NAME>

# starting containers automatically
se have to set the restart policies for the containers
to do so we use --restart flag:
no: by default
on-failure
always
unless-stopped

docker container run -d --name <NAME> --restart <RESTART> <IMAGE>
# Docker events
get real time event data about containers
we can interact with docker vents using docker API
docker system events to listen to events, and we can use  multiple filters
docker system events --since <TIMEPERIODE>
dicjer system events --filter <FILTERNAME>=<FILTREVALUE>
# Managing stopped containers
to check which containers with their ids (q) 
docker container ls -a -q
we can use a filter on that, and filter on the status
dpcker container ls -a -q -f status=exited
or you can just run prune on them
# Managing containers with portainer
portainer is a tool to manage docker as well as docker swarm that has the action of command lines in an GUI
# Updating containers with portainer
watchtower helps you watch over the container if there is an update in it, the container restars updated