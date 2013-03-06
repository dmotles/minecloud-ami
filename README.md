Minecraft-Cloud-Puppet
======================
*Build an on-demand Minecraft server on Amazon EC2.*

Minecraft-Cloud-Puppet installs and configures the Minecraft server and various management scripts to build a custom EC2 AMI--one that's designed to back up Minecraft world data to and restore world data from Amazon's S3 service upon server startup and shutdown.


Description
-----------
An effective, on-demand, Minecraft server needs more than just Java and the Minecraft server installed.

Ideally, an on-demand server should do the following:

* **Restore** your Minecraft world data from S3 backup upon server startup.
* **Save** your Minecraft world data to S3 periodically while it is running, and upon server shutdown.
* **Update** the Minecraft server when a new version becomes available.

Minecraft-Cloud-Puppet is designed to create a Minecraft EC2 AMI that does all of the above, using a set of Puppet manifests to build, install and configure the necessary software.

Despite using Puppet, you don't need to know anything about Puppet to create the AMI, since a `build-ami.py` script is included. It launches a stock Ubuntu cloud image on EC2, applies the Puppet manifests, and creates an AMI image for you. (No need to set up a Puppet Master server.)


Setup
-----
Setup is a bit involved for building your first AMI, and requires a familiarity with Amazon Web Services (AWS). The first step involves setting up and configuring your AWS account. The second step is setting up a Python virtualenv so that you can run the `build-ami.py` script.


### AWS Account ###

1. **Set up an [Amazon AWS account](https://aws.amazon.com/) for EC2 and S3.**

2. **Configure EC2**

    Using the [AWS Web console](https://console.aws.amazon.com/):

    - *Import a Key Pair*

        Upload a SSH public key called `MinecraftEC2.pub`. The `build-ami.py` script will expect the private key to be called `MinecraftEC2` and to be located in your `$HOME/.ssh` directory.

    - *Create a Security Group*

        It should be called `Minecraft` and should allow inbound traffic on two ports: 22 (SSH), and 25565 (Minecraft).

3. **Create an S3 bucket**

    Use the AWS Web interface to create an S3 bucket in which to back up your Minecraft world data files.

4. **Set environment variables**

    For AWS access:

        $ export AWS_ACCESS_KEY_ID=...
        $ export AWS_SECRET_ACCESS_KEY=...

    Name of S3 bucket to store Minecraft world data files.

        $ export MSM_S3_BUCKET=...


### Virtualenv ###

1. **Set up a virtualenv and activate it**

    I think this is easiest to do with [`virtualenvwrapper`](https://virtualenvwrapper.readthedocs.org/en/latest/):

        $ mkvirtualenv minecraft-cloud
        $ workon minecraft-cloud

2. **Clone repository**

        (minecraft-cloud)$ git clone https://github.com/toffer/minecraft-cloud-puppet
        (minecraft-cloud)$ cd minecraft-cloud-puppet

3. **Install requirements**

        (minecraft-cloud)$ pip install -r requirements.txt


Usage
-----
By default, the `build-ami.py` script will create the custom AMI in the `us-west-2` region (Oregon). If you want to use a different EC2 region, edit the `env.ec2_region` and `env.ec2_amis` variables near the top of the script.

* **Build the AMI**

        (minecraft-cloud)$ ./build-ami.py

The script takes about 30 minutes to complete. When it finishes, it will output the AMI ID.


Credits
-------
Many projects and companies have been instrumental in making this one possible. In addition to the more obvious choices (Thanks, [Amazon](https://aws.amazon.com/)! Thanks, [Puppet Labs](https://puppetlabs.com/)! Thanks, [Mojang](http://minecraft.net/)!), several other projects deserve special mention:

* [OAB-Java](https://github.com/flexiondotorg/oab-java6) by [Martin Wimpress](https://github.com/flexiondotorg).
* [Minecraft Server Manager](https://github.com/marcuswhybrow/minecraft-server-manager) by [Marcus Whybrow](https://github.com/marcuswhybrow).


License
-------
MIT License. Copyright (c) 2013 Tom Offermann.
