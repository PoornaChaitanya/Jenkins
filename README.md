# Jenkins Installation on EC2 Instance

This guide will help you install Jenkins, configure Docker as an agent, set up CI/CD.

## Create AWS EC2 Instance

1. **Go to AWS Console**
2. **Navigate to EC2 Dashboard**:
   - Services > Compute > EC2 > Instances
3. **Launch a New Instance**:
   - Click on `Launch Instance`
   - Choose an Amazon Machine Image (AMI), e.g., `Ubuntu Server 20.04 LTS (HVM)`
   - Select an instance type, e.g., `t2.micro`
   - Configure instance details, add storage, add tags if necessary
   - Configure Security Group to allow necessary ports (22 for SSH, 8080 for Jenkins, 2376 for Docker)

## Install Jenkins in AWS Instance

**Pre-Requisites: Java (JDK)**

Connect to instance using putty or Mobaxterm and run the following commands to install Java and Jenkins:

### Update package list and install Java
    sudo apt update
    sudo apt install openjdk-11-jre

### Verify Java installation
    java -version

### Add Jenkins key and repository
    curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee 
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] 
    https://pkg.jenkins.io/debian binary/ | sudo tee 
    /etc/apt/sources.list.d/jenkins.list > /dev/null

### Update package list and install Jenkins
    sudo apt-get update
    sudo apt-get install jenkins

### Configure Security Group for Jenkins

Allow TCP Port 8080:
Navigate to EC2 > Instances

Select your instance and click on the Security tab
Click on the Security Group associated with your instance
Edit inbound rules to allow All traffic on TCP port 8080
Access Jenkins

### Log in to Jenkins using the below URL:
    http://<ec2-instance-public-ip>:8080

### Retrieve the initial admin password:

    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    
Enter the password, install suggested plugins, and create an admin user if desired.

### Update package list and install Docker
    sudo apt update
    sudo apt install docker.io

### Add Jenkins and Ubuntu users to the Docker group
    sudo su - 
    usermod -aG docker jenkins
    usermod -aG docker ubuntu
    systemctl restart docker

### Restart Docker service
    sudo systemctl restart docker
    
Install Docker Pipeline Plugin in Jenkins
Log in to Jenkins
Go to Manage Jenkins > Manage Plugins

In the Available tab, search for 'Docker Pipeline'
Select the plugin and click Install
Restart Jenkins after installation
Configure Docker Agent in Jenkins

### Restart Jenkins:
    http://<ec2-instance-public-ip>:8080/restart

The docker agent configuration is now successful.
