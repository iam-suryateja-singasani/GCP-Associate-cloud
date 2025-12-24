# kubernetes
    a) most popular open source container orchestration solution
    b) provides cluster management(including upgrades), each cluster can have different types of virtual machines
    c) provides all important container orchestration features:
        1) Autoscaling
        2) service discovery
        3) load balancing
        4) self healing
        5) zero down time deployments

# GKE
    1) managed kubernetes service
    2) minimize operations with auto-repair(repair failed nodes) and auto-upgrade(use latest version of k8s always) features
    3) provides pod and cluster autoscaling
    4) enable cloud logging and cloud monitoring with simple configuration
    5) GKE engine uses container-Optimized OS, a hardened OS built by Google
    6) provides support for persistent disk and local ssd


  2: Here are some of the commands we will run in the next few steps (Refer back to this if you have any problems!)
``` bash
gcloud config set project my-kubernetes-project-304910
gcloud container clusters get-credentials my-cluster --zone us-central1-c --project my-kubernetes-project-304910
kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
kubectl get deployment
kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
kubectl get services
kubectl get services --watch
curl 35.184.204.214:8080/hello-world
kubectl scale deployment hello-world-rest-api --replicas=3
gcloud container clusters resize my-cluster --node-pool default-pool --num-nodes=2 --zone=us-central1-c
kubectl autoscale deployment hello-world-rest-api --max=4 --cpu-percent=70
kubectl get hpa
kubectl create configmap hello-world-config --from-literal=RDS_DB_NAME=todos
kubectl get configmap
kubectl describe configmap hello-world-config
kubectl create secret generic hello-world-secrets-1 --from-literal=RDS_PASSWORD=dummytodos
kubectl get secret
kubectl describe secret hello-world-secrets-1
kubectl apply -f deployment.yaml
gcloud container node-pools list --zone=us-central1-c --cluster=my-cluster
kubectl get pods -o wide
 
kubectl set image deployment hello-world-rest-api hello-world-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE
kubectl get services
kubectl get replicasets
kubectl get pods
kubectl delete pod hello-world-rest-api-58dc9d7fcc-8pv7r
 
kubectl scale deployment hello-world-rest-api --replicas=1
kubectl get replicasets
gcloud projects list
 
kubectl delete service hello-world-rest-api
kubectl delete deployment hello-world-rest-api
gcloud container clusters delete my-cluster --zone us-central1-c  
```

1) create a kubernetes cluster with default node pool
    gcloud container cluster create or use cloud console

kubernetes engine --> enable kubernetes api --> create cluste (standed(we take complete ownership of the cluster), autopilot(cluster management handeled by gke))    

**Autopilot**: new mode of operation for gke
reduce your operational cost in running kubernetes cluster
this provides an hands-off experience. Do not worry about managing the cluster infrastructure(nodes, node pool), gke will completely manage the cluster for you.

to create a cluster through console
kubernetes --> cluster base --> name of the cluster --> create

connect to the cluster through the cloud shell

```bash
gcloud config set project <projectid>
#to connect to the cluster click on three dots
gcloud cluster get-credentials my-cluster --zone us-central1-c --project mu-kubernetes-project-wdcmk
# to deploy microservices
kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
# to create a service so you can access from outside
kubectl expose deployment hello-world-rest-api --type=loadbalancer --port=8080
```

**Add node poos**: we created one node pool with 3 nods and if i want to create specific nodes with GPU, we can create multiple node pools
click on add node pools --> select the type of node you want --> and image you want

**workloads**: if you want to look what is running as part of the cluster, here you can see the deployments that we created


to increase the pods to my deployment
```bash
kubectl scale deployment hello-world-rest-api --replicas=3
kubectl get deployment
watch curl <http://nodeip:8080/hellow-world>
#to increase the number of nodes in your kubernetes cluster:
gcloud container clusters resize my-cluster --node-pool my-node-pool --num-nodes 5 
```
# autoscale kubernetes
```bash
# setup auto scaling for microservices which is also called horizontal pod autoscaling

kubectl autoscale deployment hello-world-rest-api --max=4 --cpu-percent=70

kubectl get hpa

# the above command auto scales the pods till the nodes has the resource availabe so how to autoscal nodes

gcloud container clusters update cluster-name --enable-autoscaling --min-nodes=1 --max-nodes=10

# configuration the microservice to a database.
# add some application configuration for your microservices
configmap 

kubectl create configmap <name of the config> --from-literal=RDS_DB_NAME=todos

kubectl get configmap

# if you want to store secrets like adding password configuration for your microservice
kubernetes secret

kubectl create secret generic <name of the secret> from-literal=RDS_PASSWORD=dummytodos

kubectl get secrets

kubectl describe secret <name of the secret>
```

## GKE Cluster Types

zonal cluster: 
    single zone - single control plane, nodes running in the same zone

    multi-zonal - single control plane but nodes running in multiple zones

Regional cluster: bec of master node failures 
    replicas of the controle plane runs in multiple zones of a given region. Nodes also run in same zones where control plane runs    

private cluster:
     VPC-native cluster. Nodes only have internal IP addresses    

Alpha Cluster:    to test the new kubernetes features, cluster with alpha APIs - early feature APIs.

GKE-Remember the points
===========
1) replicate master nodes across multiple zones for high availability
2) some CPU on the nodes is reserved by control plane:
    the nodes should communicate back to the master.
       a) so we should reserve 1st core-6%, 2nd core-1%, 3rd/4th core - 0.5%, Rest-0.25%

3) you can create docker image and push to GCR which is a container reposatory

4) kubernetes supports statefull deployments like kafka, Redis, Zookeeper:
    insteed of deployments use statefulset - which set of pods with unique, persistent identites and stable 
5) DaemonSet: helps to create a pod on every nodes so that if you want to run services on nodes for log collection or monitoring     
6) cloud monitoring and logging can be enabled for the clusters
    the cloud logging system and application logs can be exported to big query or pub/sub


    