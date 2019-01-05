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

The steps are the following:
1) Install Docker on the instance
2) Import a container "containing" MiniConda and essential ML libraries
