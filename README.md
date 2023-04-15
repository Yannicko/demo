In this Project, our aim is to acquire knowledge on utilizing Jenkins as a proficient Continuous Integration (CI) tool to streamline the process of integrating our source code. We will achieve this by automating the entire process from the point of code submission to our Version Control System (VCS), all the way up to the deployment of the code to our production server.

1.	Managing nodes with Jenkins.

To begin with, we need to create three Amazon Elastic Compute Cloud (EC2) instances, namely MasterServer, Test-Server, and Prod-Server, via the AWS Console. Next, we will proceed with installing Jenkins on the MasterServer by referring to the Jenkins installation documentation. Or follow these steps to install Jenkins on your MasterServer. We will do it by using the user Data option at the beginning of launching our server. Copy and past the below commands to the user data interface and launch your EC2 instance. We are going to use Bash Script.don't forget to open port 8080 for Jenkins on the Security group you will attach to your MasterServer.

```

#!/bin/bash
sudo apt update
sudo apt install openjdk-11-jdk -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y

```

After verifying the successful creation of the three instances, which will be represented by three different terminals , we will install Jenkins on the MasterServer.

Subsequently, we will move on to setting up the Jenkins Master-Slave Cluster. To initiate this process, we need to first check the status of the Jenkins Master. 

step 1: Please copy and run the command provided below

```
Service Jenkins status

```
incase the command did not work, it means you install your Jenkins through User Data at the beginning of lauching your server. so there is no need to check the status.

step 2: login to your Jenkins server using admin as user and password as password

there is a couple steps you have to go through when first open your Jenkins onyour browser, it is self-explanatory just follow the steps on the user interface of your Jenkins page and create you account. first go to the MasterServer, copy the public IP and add :8080 to open the Jenkins page on your browser 

Step 03: On Jenkins Master Dashboard –> Manage Jenkins –> Click on Configure Global Security

Step 04: Change the Agents to Random. Then click on Save.

![image](https://user-images.githubusercontent.com/97215751/232187453-2a1f2881-b982-46e8-b7f9-bf4661c19dda.png)
![image](https://user-images.githubusercontent.com/97215751/232187672-2dc4e139-9cb7-4a77-a340-5d0ca1ab6534.png)

Step 05: Now from the same Manage Jenkins –> Manage Nodes and Clouds.

![image](https://user-images.githubusercontent.com/97215751/232188084-dc525756-20e0-4149-a395-5e33050b2a09.png)

Step 06: Click on New Node. Add Test-Server as new node and make it Permanent Agent. Click on OK.

![image](https://user-images.githubusercontent.com/97215751/232188381-076ebffa-da29-4850-9dcc-c469ba010f20.png)

Step 07: Go to Launch method and make sure it’s Launch agent by connecting to the master

Step 08: Then copy /home/ubuntu/jenkins and add to the current working directory under Custom WorkDir path Then click on Save.

![image](https://user-images.githubusercontent.com/97215751/232188720-f9bb21b1-2b6c-481e-a191-828d6cb6cb7d.png)

Step 09: Create another node as Prod-Server and select copy from Test-Server as shown below:

![image](https://user-images.githubusercontent.com/97215751/232188896-5c1e7fe6-f58f-4ca2-9fa8-f99f7c25af54.png)

Step 10: Then click ok. You can see the list of nodes that we have on the Jenkins Dashboard

![image](https://user-images.githubusercontent.com/97215751/232189227-ffff19f5-9977-4aaf-a492-44db4a45d723.png)






