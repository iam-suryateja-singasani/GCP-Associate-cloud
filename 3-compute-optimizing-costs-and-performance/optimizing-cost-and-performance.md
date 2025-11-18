<!-- things can do to keep the cost of VM low -->

## Sustained use discounts

Automatic discounts for running VM instances for significant portion of the billing mont
 EG:
   1) if you use N1, N2 machine types for more than 25% of a month, you get a 20% to 50% discount on each incremental min
   2) Discount increases with usage which automatically appliesy

Applicable for instances created by GKE and Compute Engine 

Restriction: 
   1) Does not apply on certain machine types (eg: E2 and  A2)
   2) Does not apply to the VMs created by app engine flexible and Dataflow

## committed use discounts 

   1)  for workloads with predictable resource needs (if you want to run the web applications for long time)
   2)  if you need a vm for perticular type for 1 year or 3 years which is commitment giving them 
   3) Up to 70% discount based on the machine type and GPU's
   4) commited use discounts are higher than sustained use discounts
   5) Applicable for instances created by GKE and Compute Engine 
Restriction: 
   1) Does not apply on certain machine types (eg: E2 and  A2)
   2) Does not apply to the VMs created by app engine flexible and Dataflow
for commited discounts you should ask them
     compute engine --> virtual machines --> committed use discounts --> purchase commitment

## Preemptibile VM
   1) short-lived cheaper(upto 80% discount) compute instances
      can be stopped by GCP any time (oreempted) within 24 hours. you get 30 sec warning before stopping it so you can save anything if needed
    2) use preemptable VM's if:
        1) applications are fault tolerant  
        2) you are very cost sensitive
        3) your workload is not immediate (if users are accessing a specific application then your workload is immediate )
        4) non immediate batch processing jobs
    Restriction:
            not always available
            no SLA and cannot be migrated to regular VM's
            no automatic restarts
            free tier credir=ts not applicable

    to create preemptibile instance 
            compute engine --> virtual machines--> create --> manage security disk --> availability policy preemptibility

## Spot VMs
      which are latest version of preemptible vm      
      the key difference is for spot vm doesnot have maximum runtime but   preemptibile has max runtime 24 hours  

## Google compute Engine Billing      
   1) you are billed by the seconf(after a minimium of 1 min)
   2) you are not billed for compute when a compute instance is stopped. but you will be billed for any storage attached with it

   **Recommendation**: always create budget alerts and make use of budget exports to stay on top of billing
        navigation ---> Billing---> budgets & alerts

        billing export is to send the billing data to other services like bigquery

## compute Engine feature:
### Live migration & Availability Policy
    how do you keep your VM instances running when a host system needs to update (a software or a hardware update needs to be performed with out application downtime )

    Live Migration:
        1) your running instance is automatically migrated to another host in the same zone 
        2) Does not changes any attributes or properties of VM
        3) Supported for instances with local SSD
        4) Not supported for GPUs and preemptiable instances
    to configure live migration we should go to Availability Polict
        1) on host maintenance: what should happen during periodic infrastructure maintenance?
           a)  migrare(default): migrate VM instance to other hardware
            b) terminate: stop the vm instance which stops the instance at the time of migration
        2) Automatic restart: restart vm instance if there are terminated due to non-user-initiative reason(maintenance event, hardware failure etc.)

        to do this in the console you can see them at preemptiable configuration

### custom machine types
        what do you do when predefined vm option are not appropriate for your workload?
            create a machine customized to your needs(a custom machine type)
        custom machine types: adjust vCPU, memory and GPUs
            a) choose between E2, N2 or N1 machine types
            b) support a wide varety of operating systems: CentOS, coreOS, Debian, RedHat, Ubuntu, Windows etc
            c) billed per vCPU, memory provisioned to each instance

### GPU 
       a) accelerate mathe intensive and graphics-intensive workloads for AI/ML
       b) add a GPU to your vm, higher cost, use images with GPU libraries like Deep learning

       GPU restrictions:
            not supported on all machine types like shared-core or memory optimized machine types),live migration is not possible


## important points for virtual machine            
        1) VM associated with the project
        2) machine type availability can vary from region to region
        3) we can only change the machine type of a stopped instance
        4) instances are running in a zone but images are global so you can provide access to other projects 

## compute engine scenarios 
    1) pre-requisites to crweate a vm instance  ---> project, billing account, compute engine API should be enabled
    2) if you want dedicated hardware for your compliance, licensing, and management needs  ---> compute engine--> sole-tenant node
    3) how to manage 1000's of vm to automate os patch management   ---> compute engine --> os patch management
    