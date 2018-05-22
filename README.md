work-work-gcp
==============

My eyes are open.


#### Google Cloud Platform

Google Compute Engine lets you create virtual machines running different operating systems, including multiple flavors of Linux (Debian, Ubuntu, Suse, Red Hat, CoreOS) and Windows Server, on Google infrastructure. You can run thousands of virtual CPUs on a system that has been designed to be fast and to offer strong consistency of performance.


#### Activate Google Cloud Shell

From the GCP Console click the Cloud Shell icon on the top right toolbar.

This virtual machine is loaded with all the development tools you'll need. It offers a persistent 5GB home directory, and runs on the Google Cloud, greatly enhancing network performance and authentication. Much, if not all, of your work in this lab can be done with simply a browser or your Google Chromebook.

Once connected to the cloud shell, you should see that you are already authenticated and that the project is already set to your PROJECT_ID

`gcloud auth list`

`gcloud config list project`

If it is not, you can set it with this command:

`gcloud config set project <PROJECT_ID>`

<b>Note</b>: `gcloud` is the powerful and unified command-line tool for Google Cloud Platform. Full documentation is available from https://cloud.google.com/sdk/gcloud. It comes pre-installed on Cloud Shell. You will notice its support for tab-completion.


#### Understanding Regions and Zones

Certain Compute Engine resources live in regions or zones. A region is a specific geographical location where you can run your resources. Each region has one or more zones. For example, the `us-central1` region denotes a region in the Central United States that has zones `us-central1-a`, `us-central1-b`, `us-central1-c`, and `us-central1-f`.

Resources that live in a zone are referred to as zonal resources. Virtual machine Instances and persistent disks live in a zone. To attach a persistent disk to a virtual machine instance, both resources must be in the same zone. Similarly, if you want to assign a static IP address to an instance, the instance must be in the same region as the static IP.


#### Create a new instance from the Cloud Console

In the Google Cloud Platform Console, click the Menu icon on the top left of the screen.

Then navigate to <b>Compute Engine > VM Instances.</b>

To create a new instance, click <b>Create.</b>

<b>Note:</b> The instance creation process is asynchronous. You can check on the status of the task using the top right hand-side <b>Activities</b> icon. Wait for it to finish - it shouldn't take more than a minute.

Once finished, you should see the new virtual machine in the <b>VM Instances</b> page.

To SSH into the virtual machine, click on <b>SSH</b> on the right hand side. This launches a SSH client directly from your browser.


#### Install a NGINX web server

Once SSH'ed, get `root` access using `sudo`.

`sudo su -`

As the `root` user, update your OS:

`apt-get update`

Install NGINX:

`apt-get install nginx -y`

Check that NGINX is running:

`ps auwx | grep nginx`

Awesome! Let's see the web page:

Go to the Cloud Console and click the `External IP` link of the virtual machine instance. You can also see the web page by adding the `External IP` to http://EXTERNAL_IP/ in a new browser window or tab.


#### Create a new instance with gcloud

Rather than using the Google Cloud Console to create a virtual machine instance, you can use the command line tool `gcloud`, which is pre-installed in Google Cloud Shell. Cloud Shell is a Debian-based virtual machine loaded with all the development tools you'll need (`gcloud`, `git`, and others) and offers a persistent 5GB home directory.

Open the Google Cloud Shell by clicking on the icon on the top right of the screen.

Once opened, you can create a new virtual machine instance from the command line by using `gcloud`:

`gcloud compute instances create gcelab2 --zone us-central1-c`

The instance created has these default values:

- The latest Debian 9 (stretch) image.
- The `n1-standard-1` machine type. In this lab you can select one of these other machine types if you'd like: `n1-highmem-4` or `n1-highcpu-4`. When you're working on a project outside of Qwiklabs, and none of the predefined machine types meet your needs, you can also specify a custom machine type. To successfully complete this lab, you'll need to stick to the pre-defined types above.
- A root persistent disk with the same name as the instance; the disk is automatically attached to the instance.

Run `gcloud compute instances create --help` to see all the defaults.

<b>Note:</b> You can set the default region and zones that `gcloud` uses if you are always working within one region/zone and you don't want to append the `--zone` flag every time. Do this by running these commands :

`gcloud config set compute/zone ...`

`gcloud config set compute/region ...`

Finally, you can SSH into your instance using `gcloud` as well. Make sure you adjust your zone if you've used something else when creating the instance, or omit the `--zone` flag if you've set the option globally:

`gcloud compute ssh gcelab2 --zone us-central1-c`

Now you'll type <b>Y</b> to continue.

<b>Enter</b> through the passphrase section to leave the passphrase empty.

Disconnect from SSH by exiting from the remote shell:

`exit`


#### Getting Started with Cloud Shell & gcloud

Google Cloud Shell provides you with gcloud command-line access to computing resources hosted on Google Cloud Platform and available through the Google Cloud Platform Console. Cloud Shell is a Debian-based virtual machine with a persistent 5GB home directory, which makes it easy for you to manage your Cloud Platform projects and resources. With Cloud Shell, the Cloud SDK gcloud and other utilities you need are pre-installed and always available when you need them.

`gcloud auth list`

`gcloud config list project`


#### Use the command line

After Cloud Shell is activated you can use the command line to invoke the Cloud SDK gcloud command or other tools available on the virtual machine instance. You can also use your $HOME directory in persistent disk storage to store files across projects and between Cloud Shell sessions. Your $HOME directory is private to you and cannot be accessed by other users.

Let's take a look at some of the commands available.

Simple usage guidelines are available by adding -h onto the end of any gcloud invocation. Try this:

`gcloud -h`

More verbose help can be obtained by appending `--help` flag, or executing `gcloud help COMMAND`. Press Enter or the spacebar to scroll through the help content. Press `q` to exit the content.

Give it a try:

`gcloud config --help`

`gcloud help config`

You can see that the `gcloud config --help` and `gcloud help config` commands are equivalent. Both give long, detailed help.


#### Use your home directory

Now let's try out your home directory. The contents of your Cloud Shell home directory persists across projects between all Cloud Shell sessions, even after the virtual machine terminates and is restarted.

Change your current working directory:

`cd $HOME`

Open your `.baschrc` configuration file using `vi`.

`vi ./.bashrc`

The editor opens and displays the contents of the file. Press the `ESC` key and then `:q` to exit the editor.


#### Using gcloud commands

Duration is 1 min

Let's view the list of configuration in our environment. From reading the long, detailed help results in the previous step, we know we can use the command `gcloud` list.

`gcloud config list`

To check how other properties are set, see all properties by calling:

`gcloud config list --all`


#### Managing Cloud Storage data

Use the `gsutil` tool in Cloud Shell to manage Cloud Storage resources. This includes creating and deleting buckets and objects, copying and moving storage data, and managing bucket and object ACLs. `gsutil` will also let you transfer data in and out of your Cloud Shell instance.

Try creating a Cloud Storage bucket. Bucket names must be unique, so replace `unique-name` with something else, or append the name to make it unique.

`gsutil mb gs://unique-name`

Now we'll create some data to upload to your bucket.

First, create a test file:

`vi test.dat`

Start the editor:

`i`

Add some data to your file:

`Welcome to gcloud!`

Save the `test.dat` file: , then

`:wq`

Now upload some data to the bucket you created (make sure to replace "unique-name" with your storage bucket):

`gsutil cp test.dat gs://unique-name`


#### Provision Services with Cloud Launcher

Cloud Launcher provides a way to launch common software packages and stacks on Google Compute Engine with just a few clicks. Many common web frameworks, databases, CMSs, and CRMs are supported. This is one of the fastest ways to get up and running on Google Cloud Platform.


#### Getting Started with Cloud Launcher

In this section, you'll learn how to create an Nginx Stack on Google Compute Engine with Cloud Launcher.


#### Navigate to Cloud Launcher

In the Google Cloud Console, navigate to <b>Cloud Launcher</b> and click on it.


#### Choosing Nginx

In the search box that says "Search for solutions" type "Nginx". Then click on the Nginx Certified by Bitnami tile to select the platform.

Now click <b>Launch on Compute Engine.</b>


#### Launching the Nginx Stack

VM Instance Configuration

Once the project is created you'll be taken to the New Nginx deployment page in the Cloud Console to configure your Nginx instance.

On this screen you will:

- Choose a name for your instance.
- Pick a zone as us-central1-a.

Leave as default:

- Machine type: micro (1-shared vCPU) 0.6 GB memory.
- Boot Disk: 10 GB SSD.
- "Allow HTTP Traffic" and "Allow HTTPS Traffic" are checked.

Click <b>Deploy</b> to launch your Nginx Stack.

Creating the VM instance and deploying the Nginx Stack to it may take a few minutes. Cloud Deployment Manager will provide progress details.


#### Verifying the Deployment

When the Cloud Console says that your Nginx stack has been deployed you can verify that everything worked correctly. Your screen should look something like this.

Verifying via the Web

Click on the blue <b>Visit the site</b> button to access the deployed Nginx Stack in a new tab.

Verifying via SSH

You can also click on the SSH link for the VM instance in the console to open an SSH prompt in a new browser window. You can use standard Unix commands like `ps` to see if Ngnix is running on your instance.

`ps aux | grep nginx`


#### Creating a Persistent Disk

Google Compute Engine provides persistent disks for use as the primary storage for your virtual machine instances. Like physical hard drives, persistent disks exist independently of the rest of your machine – if a virtual machine instance is deleted, the attached persistent disk continues to retain its data and can be attached to another instance.

There are 2 types of persistent disks:

- Standard persistent disk
- SSD Persistent disk

Each type of persistent disks will have different capacity limits.

#### Activate Google Cloud Shell

`gcloud auth list`

`gcloud config list project`

If it is not, you can set it with this command:

`gcloud config set project <PROJECT_ID>`


#### Create a new instance

In Cloud Shell command line, use the `gcloud` command to create a new virtual machine instance named `gcelab`:

`gcloud compute instances create gcelab --zone us-central1-c`

The newly created virtual machine instance will have a default 10 GB persistent disk as the boot disk.


#### Create a new persistent disk

Because we want to attach this disk to the virtual machine instance we created in the previous step, the zone must be the same.

Still in the Cloud Shell command line, use the following command to create a new disk named `mydisk`:

`gcloud compute disks create mydisk --size=200GB \
--zone us-central1-c`


#### Attaching a disk

Attaching the persistent disk

You can attach a disk to a running virtual machine. Let's attach the new disk (`mydisk`) to the virtual machine instance you just created (`gcelab`).

Use the following command to attach the disk:

`gcloud compute instances attach-disk gcelab --disk mydisk --zone us-central1-c`

Finding the persistent disk in the virtual machine

The persistent disk is now available as a block device in the virtual machine instance. Let's take a look.

1. SSH into the virtual machine:

`gcloud compute ssh gcelab --zone us-central1-c`

2. At the prompt, enter `y` to continue.

3. When prompted for an RSA key pair passphrase, press <b>enter</b> for no passphrase, and then press <b>enter</b> again to confirm no passphrase.

4. Now find the disk device by listing the disk devices in `/dev/disk/by-id/`.

`ls -l /dev/disk/by-id/`

You found the file, the default name is:

`scsi-0Google_PersistentDisk_persistent-disk-1.`

If you want a different device name, when you attach the disk, you would specify the `device-name` parameter. For example, to specify a device name, when you attach the disk you would use the command:

`gcloud compute instances attach-disk gcelab --disk mydisk --device-name <YOUR_DEVICE_NAME> --zone us-central1-c`

Formatting and mounting the persistent disk
Once you find the block device, you can partition the disk, format it, and then mount it using the following Linux utilities:

- `mkfs:` creates a filesystem
- `mount:` attaches to a filesystem

1. Make a mount point:

`sudo mkdir /mnt/mydisk`

2. Next, format the disk with a single `ext4` filesystem using the `mkfs` tool. This command deletes all data from the specified disk:

`sudo mkfs.ext4 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1`

3. Now use the `mount` tool to mount the disk to the instance with the `discard` option enabled:

`sudo mount -o discard,defaults /dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1 /mnt/mydisk`

That's it!

Automatically mount the disk on restart

By default the disk will not be remounted if your virtual machine restarts. To make sure the disk is remounted on restart, you need to add an entry into `/etc/fstab.`

1. Open `/etc/fstab` in nano to edit.

`sudo nano /etc/fstab`

2. Add the following below the line that starts with "UUID=..."

`/dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1 /mnt/mydisk ext4 defaults 1 1`

`/etc/fstab` content should look like this:

`UUID=e084c728-36b5-4806-bb9f-1dfb6a34b396 / ext4 defaults 1 1
/dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1 /mnt/mydisk ext4 defaults 1 1`

3. Save and exit nano by pressing `Ctrl+o`, `Enter`, `Ctrl+x`, in that order.


#### Local SSDs

Google Compute Engine can also attach local SSDs. Local SSDs are physically attached to the server hosting the virtual machine instance to which they are mounted. This tight coupling offers superior performance, with very high input/output operations per second (IOPS) and very low latency compared to persistent disks.

Local SSD performance offers:

- Less than 1 ms of latency
- Up to 680,000 read IOPs and 360,000 write IOPs

These performance gains require certain trade-offs in availability, durability, and flexibility. Because of these trade-offs, local SSD storage is not automatically replicated and all data can be lost in the event of a host error or a user configuration error that makes the disk unreachable. Users must take extra precautions to backup their data.

This lab does not cover local SSDs. To maximize the local SSD performance, you'll need to use a special Linux image that supports NVMe.


#### Hello Node Kubernetes

Kubernetes is an open source project (available on kubernetes.io) which can run on many different environments, from laptops to high-availability multi-node clusters; from public clouds to on-premise deployments; from virtual machines to bare metal.


#### Activate Google Cloud Shell

`gcloud auth list`

`gcloud config list project`

If it is not, you can set it with this command:

`gcloud config set project <PROJECT_ID>`


#### Create your Node.js application

Using Cloud Shell, we'll write the application that we want to deploy to Kubernetes Engine. Here is a simple Node.js server:

`vi server.js`

Start the editor:

`i`

Add this content:

`var http = require('http');
var handleRequest = function(request, response) {
  response.writeHead(200);
  response.end("Hello World!");
}
var www = http.createServer(handleRequest);
www.listen(8080);`

We use `vi` here but `nano` and `emacs` are also available in Cloud Shell. 

Save the `server.js` file: \<Esc> then:

`:wq`

Since Cloud Shell has the `node` executable installed, run this command (the command produces no output):

node server.js

Use the built-in Web preview feature of Cloud Shell to open a new browser tab and proxy a request to the instance you just started on port 8080.

A new tab will open to display your results.

Before we continue, stop the running node server by typing <b>Ctrl-c</b> in Cloud Shell.

Now, more importantly, let's package this application in a Docker container.


#### Create a Docker container image

Next, create a `Dockerfile` that describes the image you want to build. Docker container images can extend from other existing images, so for this image, we'll extend from an existing Node image.

`vi Dockerfile`

Start the editor.

`i`

Add this content :

`FROM node:6.9.2
EXPOSE 8080
COPY server.js .
CMD node server.js`

This "recipe" for the Docker image will:

- Start from the node image found on the Docker hub.
- Expose port 8080.
- Copy our server.js file to the image.
- Start the node server as we previously did manually.

Save this `Dockerfile:` \<Esc>, then:

`:wq`

Build the image with the following, replace `PROJECT_ID` with your lab project ID, found in the Console:

`docker build -t gcr.io/PROJECT_ID/hello-node:v1 .`

It'll take some time to download and extract everything, but you can see the progress bars as the image builds. Once complete, test the image locally with the following command which will run a Docker container as a daemon on port 8080 from our newly-created container image:

`docker run -d -p 8080:8080 gcr.io/PROJECT_ID/hello-node:v1`

Your output should look something like this:

`325301e6b2bffd1d0049c621866831316d653c0b25a496d04ce0ec6854cb7998`

Take advantage of the Web preview feature of Cloud Shell to see your results.

Or use `curl` from your Cloud Shell prompt:

`curl http://localhost:8080`

This is the output you should see :

`Hello World!`

Now let's stop the running container.

Find your Docker container ID by running:

`docker ps`

Stop the container by using the CONTAINER ID provided from the previous step:

`docker stop [CONTAINER ID]`

Your console output you should look like this (your container ID):

`2c66d0efcbd4`

Now that the image is working as intended, push it to the Google Container Registry, a private repository for your Docker images, accessible from your Google Cloud projects.

Run:

Make sure to replace `PROJECT_ID` with your lab project ID, found in the Console

`gcloud docker -- push gcr.io/PROJECT_ID/hello-node:v1`

The initial push may take a few minutes to complete. You'll see the progress bars as it builds.

The container image will be listed in your <b>Console: Tools > Container Registry.</b> Now you have a project-wide Docker image available, which Kubernetes can access and orchestrate.

Note: We used a generic domain for the registry (gcr.io). You can be more specific about which zone and bucket to use. Details are documented here: https://cloud.google.com/container-registry/docs/#pushing_to_the_registry


#### Create your cluster

Now you're ready to create your Container Engine cluster. A cluster consists of a Kubernetes master API server hosted by Google and a set of worker nodes. The worker nodes are Compute Engine virtual machines.

Make sure you have set your project using `gcloud` (replace `PROJECT_ID` with your lab Project ID, found in the console):

`gcloud config set project PROJECT_ID`

Create a cluster with two n1-standard-1 nodes (this will take a few minutes to complete):

`gcloud container clusters create hello-world \
                --num-nodes 2 \
                --machine-type n1-standard-1 \
                --zone us-central1-a`

You can also create this cluster through the Console, image shown above: <b>Kubernetes Engine > Kubernetes clusters > Create cluster.</b>

It is recommended to create the cluster in the same zone as the storage bucket used by the container registry (see previous step).

Now you have a fully-functioning Kubernetes cluster powered by Kubernetes Engine.

It's time to deploy your own containerized application to the Kubernetes cluster! From now on we'll use the `kubectl` command line (already set up in your Cloud Shell environment).


#### Create your pod

A Kubernetes <b>pod</b> is a group of containers tied together for administration and networking purposes. It can contain single or multiple containers. Here we'll use one container built with your Node.js image stored in our private container registry. It will serve content on port 8080.

Let's create a pod with the `kubectl run` command (replace `PROJECT_ID` with your lab Project ID, found in the console) :

`kubectl run hello-node \
    --image=gcr.io/PROJECT_ID/hello-node:v1 \
    --port=8080`

This is the output you should see :

`deployment "hello-node" created`

As you can see, we've created a <b>deployment</b> object. Deployments are the recommended way to create and scale pods. Here, a new deployment manages a single pod replica running the `hello-node:v1` image.

To view the deployment we just created, run:

`kubectl get deployments`

This is the output you should see:

`NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   1         1         1            1           2m`

To view the pod created by the deployment, run:

`kubectl get pods`

This is the output you should see:

`NAME                         READY     STATUS    RESTARTS   AGE
hello-node-714049816-ztzrb   1/1       Running   0          6m`

Now is a good time to go through some interesting `kubectl` commands. None of these will change the state of the cluster:

`kubectl cluster-info`

`kubectl config view`

And for troubleshooting :

`kubectl get events`

`kubectl logs <pod-name>`

We now need to make your pod accessible to the outside world.


#### Allow external traffic

By default, the pod is only accessible by its internal IP within the cluster. In order to make the `hello-node` container accessible from outside the Kubernetes virtual network, you have to expose the pod as a Kubernetes service.

From Cloud Shell we can expose the pod to the public internet with the `kubectl expose` command combined with the `--type="LoadBalancer"` flag. This flag is required for the creation of an externally accessible IP:

`kubectl expose deployment hello-node --type="LoadBalancer"`

This is the output you should see :

`service "hello-node" exposed`

The flag used in this command specifies that we'll be using the load-balancer provided by the underlying infrastructure (in this case the Compute Engine load balancer). Note that we expose the deployment, and not the pod directly. This will cause the resulting service to load balance traffic across all pods managed by the deployment (in this case only 1 pod, but we will add more replicas later).

The Kubernetes master creates the load balancer and related Compute Engine forwarding rules, target pools, and firewall rules to make the service fully accessible from outside of Google Cloud Platform.

To find the publicly-accessible IP address of the service, request `kubectl` to list all the cluster services:

`kubectl get services`

This is the output you should see :

`NAME         CLUSTER-IP     EXTERNAL-IP      PORT(S)    AGE
hello-node   10.3.250.149   104.154.90.147   8080/TCP   1m
kubernetes   10.3.240.1     <none>           443/TCP    5m`

There are 2 IP addresses listed for your service, both serving port `8080`. One is the internal IP that is only visible inside your cloud virtual network; the other is the external load-balanced IP.

The `EXTERNAL-IP` may take several minutes to become available and visible. If the `EXTERNAL-IP` is missing, wait a few minutes and try again.

You should now be able to reach the service by pointing your browser to this address: `http://<EXTERNAL_IP>:8080`

At this point we've gained several features from moving to containers and Kubernetes - we do not need to specify on which host to run our workload and we also benefit from service monitoring and restart. Let's see what else we can gain from our new Kubernetes infrastructure.


#### Scale up your service

One of the powerful features offered by Kubernetes is how easy it is to scale your application. Suppose you suddenly need more capacity for your application. You can tell the replication controller to manage a new number of replicas for your pod:

`kubectl scale deployment hello-node --replicas=4`

This is the output you should see:

`deployment "hello-node" scaled`

You can request a description of the updated deployment :

`kubectl get deployment`

This is the output you should see:

`NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   4         4         4            3           16m`

You can also list the all pods :

`kubectl get pods`

This is the output you should see :

`NAME                         READY     STATUS    RESTARTS   AGE
hello-node-714049816-g4azy   1/1       Running   0          1m
hello-node-714049816-rk0u6   1/1       Running   0          1m
hello-node-714049816-sh812   1/1       Running   0          1m
hello-node-714049816-ztzrb   1/1       Running   0          16m`

We are using a <b>declarative approach</b> here. Rather than starting or stopping new instances, you declare how many instances should be running at all times. Kubernetes reconciliation loops makes sure that reality matches what you requested and takes action if needed.


#### Roll out an upgrade to your service

At some point the application that you've deployed to production will require bug fixes or additional features. Kubernetes helps you deploy a new version to production without impacting your users.

First, let's modify the application. Edit the `server.js` by starting the editor:

`vi server.js`

`i`

Then update the response message:

`response.end("Hello Kubernetes World!");`

Save the server.js file: \<Esc> then:

`:wq`

Now we can build and publish a new container image to the registry with an incremented tag (<b>`v2`</b> in this case):

Make sure to replace `PROJECT_ID` with your lab project ID, found in the Console

`docker build -t gcr.io/PROJECT_ID/hello-node:v2 .`

`gcloud docker -- push gcr.io/PROJECT_ID/hello-node:v2`

Building and pushing this updated image should be quicker as we take full advantage of caching.

Kubernetes will smoothly update our replication controller to the new version of the application. In order to change the image label for our running container, we will need to edit the existing `hello-node deployment` and change the image from `gcr.io/PROJECT_ID/hello-node:v1` to `gcr.io/PROJECT_ID/hello-node:v2.`

To do this, use the `kubectl edit` command. It opens a text editor displaying the full deployment yaml configuration. It isn't necessary to understand the full yaml config right now, just understand that by updating the `spec.template.spec.containers.image` field in the config we are telling the deployment to update the pods with the new image.

`kubectl edit deployment hello-node`

After making the change, save and close this file: \<Esc>, then:

`:wq`

This is the output you should see:

`deployment "hello-node" edited`

Run the following to update the deployment with the new image. New pods will be created with the new image and the old pods will be deleted.

`kubectl get deployments`

This is the output you should see:

`NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   4         4         4            4           1h`

While this is happening, the users of your services shouldn't see any interruption. After a little while they'll start accessing the new version of your application.

Hopefully with these deployment, scaling, and update features, once you've set up your Kubernetes Engine cluster, you'll agree that Kubernetes will help you focus on the application rather than the infrastructure.


#### Kubernetes graphical dashboard (optional)

A graphical web user interface (dashboard) has been introduced in recent versions of Kubernetes. The dashboard allows you to get started quickly and enables some of the functionality found in the CLI as a more approachable and discoverable way of interacting with the system.

To configure access to the Kubernetes cluster dashboard, from the Cloud Shell window, run these two commands (replace `PROJECT_ID` with your lab Project ID, found in the console):

`gcloud container clusters get-credentials hello-world \
    --zone us-central1-a --project <PROJECT_ID>`

To login into Kubernetes dashboard, you must authenticate using a token.

If you have not set up specific tokens for this purpose, you can use a token allocated to a service account, such as the namespace-controller.

To get the token value, try run the following command:

`kubectl -n kube-system describe $(kubectl -n kube-system \
   get secret -n kube-system -o name | grep namespace) | grep token:`
   
Copy the token as we require it for get into the Kubernetes dashboard.

`kubectl proxy --port 8081`

Then use the Cloud Shell Web preview feature to head over to port <b>8081:</b>

This should send you to the API endpoint. To get to the dashboard, remove ?authuser=0 and append the URL with "`/ui`".

Your final URL looks similar to following:

`https://8081-dot-3103388-dot-devshell.appspot.com/ui`

Select the <b>Token</b> radio button and paste the token copied from previous step. Click <b>Sign In.</b>

Enjoy the Kubernetes graphical dashboard and use it for deploying containerized applications, as well as for monitoring and managing your clusters!

You can access the dashboard from a development or local machine using similar instructions from the Web console. Click the <b>Connect</b> button for the cluster you want to monitor.
