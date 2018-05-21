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
