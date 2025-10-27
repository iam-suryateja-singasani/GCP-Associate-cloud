# google cloud engine 
   1 we can create vm using this service used to deploy our applications
   2 create and manage lifecycle of vm instances
   3 we can do load balancing and auto scaling for multiple vm instances 
   4 attach storage and network to your vm instances
   5 manage network connectivity and configuration for your vm instances

## creating vm instance with different options

1) name: vm name should provide
2) zone & Region: select zone and region where the vm insatnce should be created
3) machine configuration: which contains machine families (eg: general purpose, compute-optimized, memory-optimized, GPU e.t.c)
4) image: select the operating system eg: ubuntu
5) firewall configuration: allow http or https etc

create a simple instance with the above inputs you can connect the vm using ssh

once ssh into maching you can run some commands to play with it

``` commands to step up http server on a VM
sudo su  
apt update 
apt install apache2
ls /var/www/html
echo "Hello World!"
echo "Hello World!" > /var/www/html/index.html
echo $(hostname)
echo $(hostname -i)
echo "Hello World from $(hostname)"
echo "Hello World from $(hostname) $(hostname -i)"
echo "Hello world from $(hostname) $(hostname -i)" > /var/www/html/index.html
sudo service apache2 start

```



### compute engine machine family
   while creating vm we should take key desitions like what is hardware we are going to use and what is operating system which is the software on your specific vm

   while talking about hardware two importand things we should understand
   1) machine family
   2) machine types

      **machine family**: we have different machine families for different workloads 
         1) general Purpose (E2,N2,N2D,N1)  ---> recomended best price-performance ratio
               eg: web and application servers, small-medium databases, Dev enviroment etc
         2) memory Optimized(M2,M1) ----> ultra high memory workloads      
               large in-memory databases and in-memory analytics
         3) compute Optimized (C2): compute intensive workloads   
               Gaming applications   

      **machine type**: how much CPU, memory or disk do you want?
         varity of machine type are available for each machine family
         for eg e2-standed-2 
                  e2 --> machine type family
                  standerd ---> type of workload
                  2 - Number of CPU
         memory, disk and network capabilities increases along with vcpu's     
         
      **images**: what operating system and what software do you want on the instance?
         type of images:
            public image: provide and maintained by google and opensource communities or third party vendors
            custom images: created by you  for your projects

