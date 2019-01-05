# Use AWS' EC2 and Docker for Machine Learning

When you start a new machine learning project odds are you need to set up a new instance or server. Or you want reproduce another project but having different versions of the same libraries could lead to different results or the code might even break. In those cases Docker comes in handy. Docker is a container platform, which essentially means you can copy someone else's or your own environment that you set up for a different project including ML libraries, Jupyter notebook, Anaconda etc. and then install all of that on your instance/server. Now you're ready to go without the manual hassle and you start off at the same point.

In this repo I want to do two things.
**1) Set up a free EC2 instance on AWS and connect to it through SSH.**
**2) Use Docker to install a pre-defined environment.**

# Setting Up An EC2 Instance on AWS

The steps are the following:
1) Go to https://aws.amazon.com/, click on 'Sign In to the Console' and then log in.
2) Once you're logged in, click on 'Service' in the top-left corner for the drop-down menu. Then choose 'EC2'.

<p align="center">
  <img src="https://github.com/AleKosc/Use-AWS-EC2-and-Docker-for-Machine-Learning/blob/master/Images/instance0.PNG">
</p>

3) Next you need to download a security key from AWS which you'll need to connect to the EC2 instance. Scroll down in the left column until you reach 'NETWORK & SECURITY' and click on 'Key Pair'. Then click on 'Create Key Pair'. Save the the .pem file in the appropriate folder.

<p align="center">
  <img src="https://github.com/AleKosc/Use-AWS-EC2-and-Docker-for-Machine-Learning/blob/master/Images/key_pair1.1.PNG">
</p>

4) In the left column scroll up again to 'INSTANCES' and select 'Instances'. Then click on 'Launch Instance'. 

<p align="center">
  <img src="https://github.com/AleKosc/Use-AWS-EC2-and-Docker-for-Machine-Learning/blob/master/Images/instance1.PNG">
</p>

5) Set up the instance using Ubuntu as OS. You can use the free Ubuntu 18.04 LTS.

<p align="center">
  <img src="https://github.com/AleKosc/Use-AWS-EC2-and-Docker-for-Machine-Learning/blob/master/Images/instance2.PNG">
</p>

6) Choose the instance type. This determines the computing power. You can choose the free t2.micro.

<p align="center">
  <img src="https://github.com/AleKosc/Use-AWS-EC2-and-Docker-for-Machine-Learning/blob/master/Images/instance3.PNG">
</p>

7) You can skip steps *3.Configure Instance*, *4.Add Storage*, *5.Add Tags*, and go straight to *6. Configure Security Group*. Now you can configure which IP address can access the instance. If you're going to SSH into the instance then choose Type = SSH and you can use your own IP address. Your IP address can change when you use a different router, so you might have to reconfigure the IP address again. If you want to use Jupyter Notebook on the instance you also need to open port 8888. Right now you can choose all IP address as you don't know the IP address just yet. Once you start the Jupyter Notebook and know the IP address, you should update the security group.

<p align="center">
  <img src="https://github.com/AleKosc/Use-AWS-EC2-and-Docker-for-Machine-Learning/blob/master/Images/instance4.1_security_group.png">
</p>

8) Then choose the security key pair that you downloaded previously. 

<p align="center">
  <img src="https://github.com/AleKosc/Use-AWS-EC2-and-Docker-for-Machine-Learning/blob/master/Images/instance5.PNG">
</p>

9) Go back to *Instances* and 'Launch the Instance'.  

<p align="center">
  <img src="https://github.com/AleKosc/Use-AWS-EC2-and-Docker-for-Machine-Learning/blob/master/Images/instance5.PNG">
</p>

10) Connect to the instance via SSH. Open up the Shell or PowerShell and type in: **ssh -i /path_to_key/my_key.pem user_name@public_dns_name**. In Powershell you'll have to use "\" instead of "/". The public_dns_name becomes available when you launch an instance.

Example: **ssh -i /C:/Users/.../Desktop/GitHub/AWS_and_Docker/ec2_docker.pem ubuntu@ec2-7-5-163-17.eu-west-2.compute.amazonaws.com**

# Install Environment and ML Libraries using Docker

Now that you're logged in you can download and install Docker on your instance and then import a pre-configured container. I will import one that comes with MiniConda and some essential ML libraries.

## Install Docker on the instance

### 1) Set Up the repository

1.1) Update the apt package index:

      $ sudo apt-get update

1.2) Install packages to allow apt to use a repository over HTTPS:

      $ sudo apt-get install \
          apt-transport-https \
          ca-certificates \
          curl \
          software-properties-common

1.3) Add Docker’s official GPG key:

      $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

1.4) Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.

      $ sudo apt-key fingerprint 0EBFCD88

      pub   4096R/0EBFCD88 2017-02-22
            Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
      uid                  Docker Release (CE deb) <docker@docker.com>
      sub   4096R/F273FCD8 2017-02-22

1.5) Use the following command to set up the stable repository. You always need the stable repository, even if you want to install builds from the edge or test repositories as well. To add the edge or test repository, add the word edge or test (or both) after the word stable in the commands below.

      $ sudo add-apt-repository \
         "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
         $(lsb_release -cs) \
         stable"

### 2) Install Docker CE

2.1) Update the apt package index.

      $ sudo apt-get update

2.2) Install the latest version of Docker CE, or go to the next step to install a specific version:

      $ sudo apt-get install docker-ce

2.3) Verify that Docker CE is installed correctly by running the hello-world image.

      $ sudo docker container run hello-world

### 3) Import a container "containing" MiniConda and essential ML libraries

3.1) This command pulls the jupyter/scipy-notebook image tagged 2c80cf3537ca from Docker Hub 

      docker run -p 8888:8888 jupyter/scipy-notebook:2c80cf3537ca

    Executing the command: jupyter notebook
    [I 15:33:00.567 NotebookApp] Writing notebook server cookie secret to /home/jovyan/.local/share/jupyter/runtime/notebook_cookie_secret
    [W 15:33:01.084 NotebookApp] WARNING: The notebook server is listening on all IP addresses and not using encryption. This is not recommended.
    [I 15:33:01.150 NotebookApp] JupyterLab alpha preview extension loaded from /opt/conda/lib/python3.6/site-packages/jupyterlab
    [I 15:33:01.150 NotebookApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
    [I 15:33:01.155 NotebookApp] Serving notebooks from local directory: /home/jovyan
    [I 15:33:01.156 NotebookApp] 0 active kernels
    [I 15:33:01.156 NotebookApp] The Jupyter Notebook is running at:
    [I 15:33:01.157 NotebookApp] http://[all ip addresses on your system]:8888/?token=112bb073331f1460b73768c76dffb2f87ac1d4ca7870d46a
    [I 15:33:01.157 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [C 15:33:01.160 NotebookApp]

    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8888/?token=112bb073331f1460b73768c76dffb2f87ac1d4ca7870d46a
        
3.2) Pressing Ctrl-C shuts down the notebook server but leaves the container intact on disk for later restart or permanent deletion using commands like the following:

    # list containers
    docker ps -a
    CONTAINER ID        IMAGE                   COMMAND                  CREATED    STATUS                      PORTS               NAMES
    d67fe77f1a84        jupyter/base-notebook   "tini -- start-noteb…"   44 seconds ago    Exited (0) 39 seconds ago                       cocky_mirzakhani

    # start the stopped container
    docker start -a d67fe77f1a84
    Executing the command: jupyter notebook
    [W 16:45:02.020 NotebookApp] WARNING: The notebook server is listening on all IP addresses and not using encryption. This is not recommended.
    ...

    # remove the stopped container
    docker rm d67fe77f1a84
    d67fe77f1a84

And now you're ready to go and do your machine learning projects.
