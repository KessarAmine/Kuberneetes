# Docker netowrking
there are 3 major compononets for docker networking
1. Container netork Model (CNM)
2. libnetwork : it implements the CNM
3. Drivers(extend the model by network topologie):
    Bridge:defaulton
    host: only for linux
    overlay: mainly in docker swarm
    macvlan: assign a mac adress to a container, 
    none: disabled networking
    third-party...

CNM (defines the dundamental building block for a docker networking):
1.sandboxes: isolates the network stack (dns, ports, interfaces, routes tables) containes endpoints
2.endpoints: which allows to connect sandbox componenets to networks (with end_points)
3.networks: like we ve seen prev

# docker networking commands
{each network has a pool of ips like a subnet}

ifconfig
    <docker0> networks is a default bridge network installed with docker that binds with <ETHO> and <lo>
docker network ls
docker network inspect <NAME>
    in config we find the ip pool/subnet and the default gateway
    more info in options can be found
    internal: bool (weather the network is public and accesible from outside or not)
docker nework create <NAME>
docker network rm <NAME>
docker network prune
    risky: remove all unused networks without indicating which ones got deleted
docker network connet <NET_NAME> <CONTAINER_NAME>
docker network disconnet <NET_NAME> <CONTAINER_NAME>

# networking the containers
to craete subnets and gateways we need to work with private ip ranges and not public ip ranges which means
    10.
    192.
    172.

docker network create --subnet <SUBNET> --gateway <GATEWAY> <NAME>
                                10.1.0.0./24        10.1.0.1
docker network create --subnet <SUBNET> --gateway <GATEWAY> --ip-range=<IP_RANGE> --driver= <DRIVER> --label=<LABEL> <NAME> 

# Summary
for an app we need two networks, we have a network that communicates with dockerhost interfaces and one that is internal for the containers we use
docker nework create <NAME>
docker nework create <NAME> --internal

to create containers that work with those networks we do:
docker container run -d \
-- network=<APPROPRIATE_NETWORK>
<IMAGE>:<TAG>
    *if it is a database we use -e MY_ROOT_PASSWORD=<PASSWORD>

to connect the networks with containers we do:
docker network connet <NET_NAME> <CONTAINER_NAME>
