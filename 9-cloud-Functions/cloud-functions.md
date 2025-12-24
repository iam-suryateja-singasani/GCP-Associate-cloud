# Cloud Functions
1) Imagine you want to execute some code when an event happens?
    A file is uploaded to cloud storage (or)
    an error log is written to cloud logging (or)
    a message arrives to cloud Pub/Sub
    a http/https invocation is received
2) Run code in response to events
       a) write your business logic in node.js, python, go, java, .net and ruby
       b)don't worry about servers or scaling or availability (only worry about code)
3) Pay only for what you use
    number of invocations
    compute time of the invocations
    memory and cpu provisioned
4) time bond - default 1 min and max 60 mins in 2nd gen for https request

5) 2 product versions
    a) cloud functions (1st gen): first version
    b) cloud functions(2nd gen): new versions build to top of cloud run and eventarc

## important concepts in cloud functions are    

**events**: is for eg uploading object to cloud storage
**trigger**: responce to event with a function call
    **trigger**: which function to trigger when an event happens?
    **functions**: take event data and perform action?
events are triggered from:
    a) cloud storage
    b) cloud Pub/Sub
    c) HTTP POST/GET/DELETE?PUT/OPTIONS
    d) FireBase
    e) Cloud Firestore
    f) Stack drive logging    

# cloud-Functions-gen2    
    Cloud Function - Generation 2 uses Cloud Run in the background!
    
google cloud recomends to use cloud function 2nd gen which can run longer up to 60 min for http-triggered functions it has capibility of larger instance size up to 16 gb ram with 4vcpu and it can handle upto 1000 concurrent request per function instance, multiple function revisions and traffic splitting supported
support for 90+ event types- enabled by eventarc

**important points in scaling and concurrency using cloud functions**
    typical serverless function architecture:
        1)autoscaling: as a new invocations come in, new function instances are created
        2)one function instance handles only one request at a time
        3) 3 concurent functions invocation ,creates 3 function instances 
            a) if a 4th function invocation occurs while existing invocationsa re in progress, a new function instances will be created 
            b) however, a function instance that completed execution may be reused for function requests
            c) **cold start**: new function instance initilization from scratch can take time for this the solution is to configure or maintain minimum number of instances (but increases cost) 

    1st get uses the typical serverless functions arcitecture
    2nd gen adds a very important new feature which is one function can handle multiple requests at the same time  which is called concurrency like how many concurrent invocations can one function instance handle? max 1000. one important thing to remember is your function code should be safe to execute concurrently

cloud function - deployment using gcloud
