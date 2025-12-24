# Cloud Run & Cloud run for Authos
    a) built on top of an open standard - knative
    b) fully manages serverless platform for containerized applications
        1) zero infrastructure management
        2) pay-as-use(for used CPU, memory, requests and networking)
fully integrated end-to-end development experience:
    a) No limitations in languages, binaries and dependencies
    b) easily portable bec of container based architecture
    c) cloud code, cloud build, cloud monitoring and cloud logging integrations
**Cloud run**: if you have a container to deploy you can go to production in seconds.
    a) cpu is only allocated during request processing(container creats only when request comes)
    b) cpu is always allocated(means your container is always running)

termenologies: 
    we create service inside service we have multiple containers revisions   

in addition to cloud run we have Anthos
  Anthos runs kubernetes cluster anywhere like if you want to run kubernetes clusters in your datacenters or in any other cloud or multi cloud we can use    

  cloud run for anthos enables to deploy your workload to anthos clusters running on-premises or on google cloud

  cloud-run cli

  Deploy a new container
    gcloud run deploy <service-name> --image <image_url> --revision-suffix v1
        first deployment creates a service and first revision
        next deployment for the same service creates new revisions
  list available revisions: 
        gcloud run revisions list

  adjust traffic assignments
        gcloud run services update-traffic myservice --to-revisions=v2=10,v1=90            