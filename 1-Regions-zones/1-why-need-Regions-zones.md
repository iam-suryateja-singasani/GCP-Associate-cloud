# why do we need regions and zones
imagine that your application is deployed in a datacenter in london region

challenges?
    1. slow access for the users from other parts of the world (high latency)
    2. if data center crashes, application goes down (low availablity)
       Add another datacenter in london will solve the problem but if entair london region crashes so we need multiple regions if london region crashed the application still available with other region and the latency problem also solved with this     

Google provides 20+ regions around the world

Region: specific geographical location to host your resources which gives high availability, low latency      

Zones: with in each region there are multiple zones in google cloud each region has atleast 3 zones which increatses availablity and fault tolerance within same region each zone has one or more discrete clusters 
    cluster: distinct physical infrastructure that is housted in the data center
    zones in a region are connected through low-latency links which means even if you deploy your application in zone 1 and DB in zone 2 you will still get a good performance



 region --> us-west1 --> 3 zones us-west1-a,us-west1-b, us-west1-c
