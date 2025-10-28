# Google Cloud Compute Engine Notes

## Overview

**Google Cloud Compute Engine (GCE)** allows you to:
1. Create and manage **Virtual Machines (VMs)** to deploy applications.  
2. Manage the **lifecycle** of VM instances.  
3. Implement **load balancing** and **auto-scaling** across multiple VM instances.  
4. Attach **storage** and **network** configurations to VM instances.  
5. Manage **network connectivity** and configuration for VMs.

---

## Creating a VM Instance

### Key Configuration Options

1. **Name** – Provide a name for the VM instance.  
2. **Zone & Region** – Select where the VM instance should be created.  
3. **Machine Configuration** – Choose the machine family (e.g., General Purpose, Compute-Optimized, Memory-Optimized, GPU).  
4. **Image** – Select the operating system (e.g., Ubuntu).  
5. **Firewall Configuration** – Allow HTTP or HTTPS access as required.

Once the instance is created, connect via **SSH** and execute commands to set up an HTTP server.

---

## Setting Up an HTTP Server on a VM

```bash
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
echo "Hello World from $(hostname) $(hostname -i)" > /var/www/html/index.html
sudo service apache2 start

```

## Compute Engine Machine Family

When creating a VM, consider:

**Hardware** (Machine Family & Type)

**Software** (Operating System/Image)

1. **Machine Families**

| Family                | Examples        | Description                      | Use Cases                                                 |
| --------------------- | --------------- | -------------------------------- | --------------------------------------------------------- |
| **General Purpose**   | E2, N2, N2D, N1 | Balanced price-performance ratio | Web/app servers, small-medium databases, dev environments |
| **Memory Optimized**  | M1, M2          | Ultra-high memory                | Large in-memory databases, analytics                      |
| **Compute Optimized** | C2              | High compute performance         | Gaming, compute-intensive workloads                       |

2. **Machine Types**

Machine types determine the **CPU, memory, and disk** resources.
Example: e2-standard-2

   e2 → Machine family

   standard → Type of workload

   2 → Number of CPUs

💡 As vCPUs increase, so do memory, disk, and network capabilities.   
         
3. **images**: 

Defines the operating system and preinstalled software on the VM.

   **Public Images** – Provided by Google, open-source communities, or third-party vendors.

   **Custom Images** – Created by you for your project needs.what operating system and what software do you want on the instance?
         
         

### Internal and external IP Adresses

   **Internal IP**: Used within a private network. Can be duplicated across different organizations.

   **External IP**: Public, unique, and accessible from the internet.

**Key Points**:

   All VMs get at least one internal IP.

   You can attach an external IP for public access. (Note: External IP changes when you stop the VM unless static.)

**Static IP Addresses**: how do you get a constant external IP address for a VM?
      quick and dirty way is to assign an static IP adress to the VM. 
      Must be in the same region as the VM.

         static IP can be switched to another VM instance in same project.
         Static IP remains attached even if you stop the instance. you have manually detach it and you will get bill for static IP when you are not using it as well!

**Simplifying VM Setup**
---> there are lot for steps to setup simple http server in a VM, to reduce the number of steps in creating and setting up a https server 
we have multiple options

1) **startup script**
2) **instance template**
3) **custom image**
   
1) **startup Script** : bootstrapping with startup script

   **Bootstrapping** means automatically installing OS patches or software during VM launch.
   You can add a startup script that runs when the instance starts.

   Example startup script:

   ```bash
   apt update
   apt install -y apache2
   echo "Hello World from $(hostname) $(hostname -i)" > /var/www/html/index.html
   sudo service apache2 start
   ```

2) **instance Template**: 

   If you want to launch multiple instances easily:

   Define details like image, machine type, labels, startup script, etc.

   Templates are immutable (cannot be edited). To modify, copy and create a new version.

   You can specify an image family to always use the latest, non-deprecated image.

   **Use Cases**:

      Create VMs quickly.

      Deploy Managed Instance Groups (MIGs).


      insteed of image you can spefy image family so the template will take latest non-depricated version of the family 

3) **Reducing Launch Time with Custom Image**
         installing OS patches and software at launch of VM instance increases boot up time.

         creating a custom image with OS patches and software pre-installed

        **Steps**:

            Create and configure a VM.

            Create an image from its persistent disk.

            Use that image to launch future VMs.

            **Best Practices**:

               Never create an image from a running instance’s disk.

               Prefer regional or multi-regional storage for availability.

               Deprecate old images and specify a replacement image.

               Commonly used to create hardened (golden) images compliant with corporate standards.

               Example modification for VMs launched from a custom image:

               ```bash
               echo "Hello world from $(hostname) $(hostname -i)" > /var/www/html/index.html
               sudo service apache2 start
               ```
### Comparison: Startup Script vs Instance Template vs Custom Image

| Feature              | **Startup Script**                                             | **Instance Template**                                                                              | **Custom Image**                                                                        |
| -------------------- | -------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Purpose**          | Automate software installation or configuration at VM startup. | Define reusable configuration for VM creation (machine type, image, labels, startup script, etc.). | Create a preconfigured image (OS + software) to launch VMs faster.                      |
| **Scope**            | *Inside the VM* – runs every time it boots.                    | *Infrastructure level* – defines how to create identical VMs.                                      | *Image level* – defines what’s already installed on the VM before launch.               |
| **Where Defined**    | In the **metadata** of the VM or template.                     | In the **Compute Engine console** (template settings).                                             | In the **Compute Engine Images section**, created from a disk/snapshot.                 |
| **When It Runs**     | During boot of each VM.                                        | When creating VM(s) from the template.                                                             | Used as the base OS image when launching VM(s).                                         |
| **Performance**      | Slowest – installs and configures during startup.              | Faster – all setup info pre-specified, but startup script may still run.                           | Fastest – everything (OS, patches, software) is already baked into the image.           |
| **Reusability**      | Needs to be attached manually to each VM or template.          | Highly reusable – can create hundreds of identical VMs easily.                                     | Highly reusable – image can be shared across projects or reused for multiple templates. |
| **Maintenance**      | Script updates must be handled manually.                       | Immutable – can’t edit; must copy and version.                                                     | Easy to version and deprecate old images.                                               |
| **Best For**         | Quick automation or testing setup.                             | Consistent, large-scale VM deployment.                                                             | Production-grade, hardened, preinstalled configurations.                                |
| **Example Use Case** | Install Apache and start service at boot.                      | Create a pool of identical web servers (MIGs).                                                     | Deploy preconfigured secure web servers instantly.                                      |




**Summary**

   Compute Engine lets you deploy scalable and customizable virtual machines.

   Choose the machine family and image according to your workload.

   Use startup scripts, instance templates, or custom images to automate setup.

   Manage internal/external IPs, and prefer static IPs for consistent connectivity.