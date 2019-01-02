# Use AWS' EC2 and Docker for Machine Learning

When you start a new machine learning project odds are you need to set up a new instance or server. Or you want reproduce another project but having different versions of the same libraries could lead to different results or the code might even break. In those cases Docker comes in handy. Docker is a container platform, which essentially means you can copy someone else's or your own environment that you set up for a different project including ML libraries, Jupyter notebook, Anaconda etc. and then install all of that on your instance/server. Now you're ready to go without the manual hassle and you start off at the same point.

In this repo I want to do two things.
1) **Set up a free EC2 instance on AWS and connect to it through SSH.**
2) **Use Docker to install a pre-defined environment.**

# Setting Up An EC2 Instance on AWS

The steps are the following:
1) Download a security key from AWS which you'll need to connect to the EC2 instance
2) Set up the instance using Ubuntu as OS and the security groups, i.e. who can access the instance
3) Connect to the instance via SSH

# Install Environment and ML Libraries using Docker

The steps are the following:
1) Install Docker on the instance
2) Import a container "containing" MiniConda and essential ML libraries
