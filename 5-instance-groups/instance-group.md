# Instance Groups --> to create group of instances as a single entity

**manage groups**: of similar vm's having similar lifecycle as one unit

two types of instance groups:
    **Managed(MIG)**: identical VM's created using a template
        features: Autoscaling, auto healing and managed releases
    **Unmanaged**: different configuration for vm's in same group:
        Does not offer autoscaling, auto healing and others services, not recomended unless you need different kinds of vm's

 we can create instance groups in zonal or regional  
    regional is recomended bec it gives high availability     

## Managed Instance Groups(MIG) 
   
    1)identical VMs created using an instance template
    2) can maintain certain number of instances, if a instance crashes mig will launch another instance
    3) can detect application failures using health checks (self healing)
    4) increase and decrease instances based on loaad(auto healing)
    5) add loadbalancer to distribute load between instances
    6) create instances in multiple zones(regional MIGs)
            this regional MIGs provide higher availability compared to zonal MIGs
    7) can release new application versions without downtime 
      1) rolling updates release new versions step by step (gradually). update a percentage of instances to the new version at a time  
      2) canery deployment test new version with a group of instances before releasing it accross all instances     

to **create** MIG what we need?
1) instance template is mandatory
2) configure auto-scaling to automtically adjust number of instances based on load:
   a) minimum number of instances
   b) maximum number of instances
   c) Autoscaling metrics: cpu utilization target or load balancer utilization target or any other metrics from stack drive 
       1) cool-down period: how long to wait before looking at autoscaling metrics again
       2) Scale in controls: prevent a sudden drop in on of VM instances, reduce no of VM instances when load decreases
            example: doesn't scale in by more than 10% or 3 instances in 5 min
       3) scale out: increase no of vm instances when load increases     
    d) Auto healing: configure a health check with initial delay(how long should you wait for your app to initialize before running a health check?)
compute engine  --> instance groups --->   new managed instance group (two types stateful(db) and stateless) --> template -->location(multy zones) --> autoscalling(on:to add anf remove vm's, on scale:to only add instances, off: not to autoscale at all)

# Updating a managed instance group (MIG)
  **Rolling Update** Gradual update of instances in an instance group to the new instance template
  1) Specify new template: specify a template for canery testing
  2) Specify how you want the update to be done:
        when should the update happen?
            stat the update immediately --> proactive
           or when the instance group is resized later(Opportunistic)
        How should the update happen?
            **max surge**: how many instances are added at any point of time
            **max unavailability** how many instances can be offline during the update?
    **Rolling Restart/replace** gradual restart or replace of all the instances in the group
        No chnage in template but replace/restart existing VMs
        configure maximum surge, maximum unavailable and what you want to do (restart/replace)         


instancd group --> update vm --> change instance template  ---> lecture 62





In GCP when you specify the name of the instance template without region details, it is searched under global scope.

gcloud compute instances create my-test-vm --source-instance-template=my-instance-template-with-custom-image
However, the instance templates are no longer available in global scope but in the specific region.



So instead of just providing the name of the template we need to provide the URL in the specific format

gcloud compute instances create my-test-vm --source-instance-template=https://compute.googleapis.com/compute/v1/projects/<<PROJECT_ID>>/regions/<<REGION>>/instanceTemplates/<<INSTANCE_TEMPLATE_NAME>>


So if your project name is just-lore-1234 and your region is us-central1 the command should be

gcloud compute instances create my-test-vm --source-instance-template=https://compute.googleapis.com/compute/v1/projects/just-lore-1234/regions/us-central1/instanceTemplates/my-instance-template-with-custom-image



```bash

gcloud compute instances create my-test-vm --source-instance-template=my-instance-template-with-custom-image
gcloud compute instance-groups managed list
gcloud compute instance-groups managed delete my-managed-instance-group
gcloud compute instance-groups managed create my-mig --zone us-central1-a --template my-instance-template-with-custom-image --size 1
gcloud compute instance-groups managed set-autoscaling my-mig --max-num-replicas=2 --zone us-central1-a
gcloud compute instance-groups managed stop-autoscaling my-mig --zone us-central1-a
gcloud compute instance-groups managed resize my-mig --size=1 --zone=us-central1-a
gcloud compute instance-groups managed recreate-instances my-mig --instances=my-mig-85fb --zone us-central1-a
gcloud compute instance-groups managed delete my-managed-instance-group --region=us-central1

```

