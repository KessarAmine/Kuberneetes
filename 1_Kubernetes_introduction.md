# INTRODUCTION

Its a cloud native application, application orchestrator
Cloud providers use Kubernetes to manage their cloud native apps even on-premises

the Kibernetes Primer is the Kubernetes cluster

# Kubernetes Primer
Kubernetes is the layer above the infra layers (VMs)... to orchester/run cloud native-apps, it is made of a bunch lf linux nodes
it is devided into two categories, contorl plane and workers
# control plane
it is the api server, the scheduler, controlers and perssistant store
..etcd is a stateful store (not a real good approach for big scale solution) don a lot of scale testing cuz it is distributed storage....
..api it is the gataway into the cluster, requests get sent to the api server with kubectl that send that request to the api, api server can talk with othe control panel nodes as well, responses will be by deploying yaml files
# worker nodes
the apps run on those nodes

# Kubernetes API
it is everything where in kubernetes is defined as ressources in the API, it is REST api that uses CRUD, most of the communication witht he API is with kubectl
Kubernetes API uses Decalrative configuration for versioning
there are differernt groups in the API:
core, apps, auth, storage..., these groups are looked after SIG, each SIG is developped and managed by one SIG
there Are mainly 3 steps of deployments:
ALPHA: HAIRY! user with caution
BETA: becoming stable
GA: reading for production

# Declarative configuration
the way of deploying anad managing  in kubernetes is:
define this different ressources and part of the apps in yaml files
post them to the API server as a record of intent, this record of intent updates the desired state of the kubernetes cluster
in the backgorund control plane is running watchloops to compare the desires state of the cluster is the same as the current state of the cluster
control panel makes changes to make the two states simillar

# Kubernetes objects
POD: Kubernetes wraps containers in a pod to run them, a pod contains one or more containers its an atomic unit scheduling
DEPLOY: defined in apps/v1 API gorup, scaling, rolling updates
DS: DAEMONSETS, makes sure that only one of a specefic Pod is runing on every worker in the cluster
srs: STATEFUL SETS for pods or parts of the app  with perticular requirements
...
