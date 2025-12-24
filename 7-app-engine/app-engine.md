# App Engine
1) simplest way to deploy and scale your application in GCP, provides end-to-end application management
2) Supports go, java, .net, node.js, php, python, Ruby using pre-configured runtime
3) we can use custom run-time and write code in any language.
4) connect to variety of google cloud storage products(Cloud SQL etc)
5) no usage charges for app engine but should pay for the resource provisioned
**Features**:
1) automatic load balancing and auto scaling
2) managed platform updates and application health monitoring
3) application versioning
4) traffic splitting from different versions

## compute engine vs app engine

compute Engine                           
 IAAS                                       
 More flexible
 More Responsibility
    choosing image
    install software
    choosing hardware
    fine grade access/permissions
    availability

App Engine    
  PAAS
  serverless
  lesser Responsibility
  lower flexibility (GPU usage not possiable etc) 


## App Engine enviroments  
 two type of enviroments
**standerd**: applications run in language specific sandboxes. so you have complete isolation from OS/DISK/Other Apps which comes in 2 versions
    1) v1 supports java, python, php, go(old versions).
       For python and php runtimes there are some restricted network access and onlu white-listed extensions and libraries are allowed
       no restrictions for java and go runtime
    2) v2: which is a newer version of standed env 
            java, python,PHP, node.js, ruby, go(newer versions)
         full network access and no restrictions on language extensions   
**flexible**: application instances run within docker containers. here we use compute engine virtual machines. supports any runtime (with built-in support for python, java, node.js, go, ruby, php, .net). provides access to background processes and local disk         

## App Engine - application component hierarchy
    there are three components in app engine

**Application**: can create one application per project.application is container for every thing you would create as part of your app engine for eg i want to deploy 3 microservices so i should create 1 application in a specific project under that application i can created differnt survices for each of the microservice
**services**: multiple microservices or app components, you can have multiple services in a single application, each service can have different settings, earlier services also called modules
**versions**: under services we can have multiple microservices each version associated with code and config
    each version can run in one or more instances, multiple version can co-exist, we have options to rollback and split traffic

## app enging scaling instances

**Automatic**: automatically scal instances based on load which is recomended for continuously running workloads
    auto scal based on:
        **tagret CPU Utilization**: configure a CPU usage threshold.
        **target throughput utilization**: configure a throughput thresould
        **max concurrent requests**: configure max concurrent request as instance can receive
    we can configure max instances and min instances
**Basic**:  instances are created as and when requests are received:
Recomended for adhoc workloads
instances are shutdown if zero requests, try to keep cost low and high latency is possiable       
not supported by app engine flexible enviroment
configure max instances and idle timeout

**Manual**: configure specific number of instances to run
adjust number of instances manually over time


```bash

cd default-service
gcloud app deploy
gcloud app services list
gcloud app versions list
gcloud app instances list
gcloud app deploy --version=v2
gcloud app versions list
gcloud app browse
gcloud app browse --version 20210215t072907
gcloud app deploy --version=v3 --no-promote
gcloud app browse --version v3
gcloud app services set-traffic split=v3=.5,v2=.5
gcloud app services set-traffic splits=v3=.5,v2=.5
watch curl https://melodic-furnace-304906.uc.r.appspot.com/
gcloud app services set-traffic --splits=v3=.5,v2=.5 --split-by=random
 
cd ../my-first-service/
gcloud app deploy
gcloud app browse --service=my-first-service
 
gcloud app services list
gcloud app regions list
 
gcloud app browse --service=my-first-service --version=20210215t075851
gcloud app browse --version=v2
gcloud app open-console --version=v2
gcloud app versions list --hide-no-traffic

```