# GCP CLI Commands

## Overview
This document covers essential Google Cloud Platform (GCP) CLI commands using `gcloud` and Cloud Shell.

there are different cli tools for GCP 
   1) cloud storage - gsutil
   2) cloud BigQuery - bq
   3) cloud Bigtable - cbt
   4) kubernetes - kubectl 

we can install gcloud using cloud SDK which requires Python
or we can use gcloud on cloud shell   

cloud shell
===========
```bash
gcloud --version
gcloud init --> to setup configeration like project
###list
gcloud config list --> list of all properties of the active config
gcloud config list account --> so core is the default and account details comes under core so it ia able to list
gcloud config list compute/region
gcloud config list core/account 
###set
gcloud config set core/project Value
gcloud config set core/verbosity Value  --> debug
gcloud config set compute/region Value
gcloud config set core/zone Value  --> debug

#Syntax: gcloud config set SECTION/PROPERTY VALUE
    #SECTIONS ---> core, compute  --> core is default section so not need to specify eg gcloud config set project value or gcloud config set core/project value
    #PROPERTIES ---> project, region, zone

gcloud config set --help

# set is used to add the default values to the like project 
# unset used to remove the default value

# gcloud--> manage multiple configurations
#========================================
# if you are working on multiple projects from the same machine. you would want to be able to execute commands using different configurations.
    gcloud config configurations create/delete/describe/activate/list
    # to get the all the list of configurations
    gcloud config configurations list
    # to create new config and activate 
    gcloud config configurations create my-second-configeration
    # to list the active configuration
    gcloud config list
    # to set the default project to active config which is my-second-configeration or set the region and zone
    gcloud config set project <project-id>
    # to switch to my-default config 
    gcloud config configurations activate my-default
    # to describe the configuration
    gcloud config configurations describe my-second-configuration
```

## gcloud command structure-playing with services

gcloud **GROUP** **SUBGROUP** **ACTION**
    **GROUP** -> config or compute or container or dataflow or functions or iam etc
    **SUBGROUP** -> instances or images or instance-templates or machine-types or regions or zones
    **ACTIONS** --> create or start or list or stop or describe

Eg: gcloud compute instances list
    gcloud compute instances create
    gcloud compute instance describe
    gcloud compute machine-types --filter= "zone:asia-southeast2-b"
    gcloud compute machine-types --filter="zone:(asia-southeast2-b asia-southeast2-c)"
```bash
# Configuration Management
gcloud config list project                                      # List project-specific configuration
gcloud config configurations list                               # List all configuration sets
gcloud config configurations activate my-default-configuration  # Switch to specified configuration
gcloud config list                                             # Show all current configuration settings
gcloud config configurations describe my-second-configuration   # Show details of specific configuration

# Compute Instance Operations
gcloud compute instances list                                   # List all VM instances in project
gcloud compute instances create                                # Create VM instance (requires name and options)
gcloud compute instances create my-first-instance-from-gcloud   # Create VM with specified name
gcloud compute instances describe my-first-instance-from-gcloud # Show detailed instance information
gcloud compute instances delete my-first-instance-from-gcloud   # Delete specified VM instance

# Resource Listing and Filtering
gcloud compute zones list                                       # List all available zones
gcloud compute regions list                                    # List all available regions
gcloud compute machine-types list                              # List all machine types
gcloud compute machine-types list --filter zone:asia-southeast2-b                    # Filter by single zone
gcloud compute machine-types list --filter "zone:(asia-southeast2-b asia-southeast2-c)" # Filter by multiple zones
gcloud compute zones list --filter=region:us-west2             # Filter zones by region
gcloud compute zones list --sort-by=region                     # Sort zones by region (ascending)
gcloud compute zones list --sort-by=~region                    # Sort zones by region (descending)
gcloud compute zones list --uri                                # Show resource URIs instead of names
gcloud compute regions describe us-west4                       # Show detailed region information

# Instance Templates
gcloud compute instance-templates list                         # List all instance templates
gcloud compute instance-templates create instance-template-from-command-line # Create new instance template
gcloud compute instance-templates delete instance-template-from-command-line # Delete specified template
gcloud compute instance-templates describe my-instance-template-with-custom-image # Show template details
```
```
## Authentication
```bash
# Login to GCP
gcloud auth login

# Set active account
gcloud config set account [EMAIL]

# List authenticated accounts
gcloud auth list
```

## Project Management
```bash
# List projects
gcloud projects list

# Set active project
gcloud config set project [PROJECT_ID]

# Get current project
gcloud config get-value project
```

## Compute Engine
```bash
# List VM instances
gcloud compute instances list

# Create VM instance
gcloud compute instances create [INSTANCE_NAME] --zone=[ZONE]

# Stop instance
gcloud compute instances stop [INSTANCE_NAME] --zone=[ZONE]

# Start instance
gcloud compute instances start [INSTANCE_NAME] --zone=[ZONE]
```

## Cloud Storage
```bash
# List buckets
gsutil ls

# Create bucket
gsutil mb gs://[BUCKET_NAME]

# Copy files
gsutil cp [SOURCE] gs://[BUCKET_NAME]/

# Sync directories
gsutil rsync -r [LOCAL_DIR] gs://[BUCKET_NAME]/
```

## Configuration
```bash
# View current config
gcloud config list

# Set compute zone
gcloud config set compute/zone [ZONE]

# Set compute region
gcloud config set compute/region [REGION]
```