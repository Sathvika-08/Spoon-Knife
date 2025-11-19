JENKINS
(tools)
JDK
→ Name: JDK
→ Add path: C:\Program Files\Microsoft\jdk-17.x\

Maven
→ Name: Maven_3.9
→ Let Jenkins auto-install 


BUILD PROJECT
master to main(change)
build steps 1)clean 
2)install
post build steps archive artifact **/*

PIPELINE
CREATE PIPELINE JOB
Step 1: Jenkins → New Item → Name: MavenJava_Pipeline
Step 2: Add this Jenkinsfile script

pipeline {
    agent any

    tools {
        maven 'Maven_3.9'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',url: 'https://github.com/Sathvika-08/maven-hello.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Run Java Program') {
            steps {
                bat 'java -cp target/hello-maven-1.0-SNAPSHOT.jar com.example.App'
            }
        }
    }
}

VIEW PIPELINE 


MINIKUBE

ashimahendra06@gmail.com
Ashi0659$

docker login
minikube start --driver=docker
docker push ashimahendra06/helloimage:latest
docker pull ashimahendra06/helloimage
kubectl create deployment demo1 --image=ashimahendra06/helloimage:latest
kubectl get pods
kubectl describe pod demo1-7f9844467c-m4lcf
kubectl expose deployment demo1 --type=NodePort --port=80
kubectl get svc
kubectl port-forward svc/demo1 7777:80



NAGIOS
docker login
docker pull jasonrivers/nagios:latest
docker run --name nagioslab -p 8082:80 jasonrivers/nagios:latest
http://localhost:8082
login creds:
nagiosadmin
nagios

AWS
# Deployment of index.html Using EC2 Instance (Ubuntu VM)

## *Step 1: Open AWS Academy Portal*

* Go to *Modules*.
* Scroll down and select *Launch AWS Academy Lab*.

## *Step 2: Start AWS Lab*

* Click *Start Lab*.
* When lab starts, click *AWS* to open the AWS Console.

## *Step 3: Open EC2 Service*

* In the AWS Console, search for *EC2*.
* Click on *EC2*.

## *Step 4: Launch an EC2 Instance*

* Click *Instances*.
* Select *Launch Instance*.

## *Step 5: Configure the Instance*

### *5.1 Instance Name*

* Enter: webapp (or any name).

### *5.2 Choose OS*

* Choose *Ubuntu Server*.
* Architecture: *64-bit (x86)*.

### *5.3 Instance Type*

* Select *t3.micro* (free tier eligible).

## *Step 6: Create Key Pair*

* Click *Create New Key Pair*.
* Key pair type: *RSA*.
* Format: *.pem*.
* Click *Create Key Pair*.
* Save the .pem file on your system.

## *Step 7: Configure Network (Security Group)*

* Click *Create Security Group*.
* Give a name.
* Click *Edit*.
* Add rule:

  * *Type:* SSH
  * *Port:* 22
  * *Source:* Anywhere (0.0.0.0/0)

## *Step 8: Storage Configuration*

* Set storage to *8 GB*.
* Number of instances: *2* (if required).

## *Step 9: Launch the Instance*

* Click *Launch Instance*.
* A success message will appear.

## *Step 10: Connect to the Instance*

* Go to *Instances*.
* Select the created instance.
* Click *Connect*.
* Select *SSH Client*.
* Copy the SSH command provided.

## *Step 11: Connect Using Terminal*

1. Open *Command Prompt/Terminal*.
2. Navigate to the folder containing your .pem file.
3. Run the SSH command:


ssh -i "yourfile.pem" ubuntu@<EC2-Public-IP>


## *Step 12: Switch to Root User*


sudo su


## *Step 13: Install Dependencies*

### *13.1 Update Package List*


sudo apt-get update


### *13.2 Install Docker*


sudo apt-get install docker.io


### *13.3 Check Installed Versions*


docker --version
git --version


## *Step 14: Clone Your GitHub Repository*


git clone <your-github-repo-url>


* Example:


git clone https://github.com/username/project.git


* Navigate to the project folder:


cd project


## *Step 15: Create or Verify Dockerfile*

### If Dockerfile does NOT exist:


nano Dockerfile


Add this sample Dockerfile:


FROM tomcat:9
COPY ./index.html /usr/local/tomcat/webapps/ROOT/index.html


Save using:

* *CTRL + O*, press Enter
* *CTRL + X* to exit

## *Step 16: Build Docker Image*


sudo docker build -t img1 .


## *Step 17: Run Docker Container*


sudo docker run -d -p 8081:8080 img1


(8081 is the EC2 external port → maps to container’s 8080)

## *Step 18: Check Docker Status*


sudo docker ps
sudo docker images


## *Step 19: Access the Deployed Application*

* Get your EC2 *Public IP* from AWS EC2 Dashboard.
* Open in browser:


http://<Public-IP>:8081


Example:


http://13.222.21.231:8081


You should see your *index.html* running live.

---

# 🎉 Deployment Successful!
SECTION 8 — EMAIL CONFIGURATION

Manage Jenkins → Configure System

Find Email Notification:

SMTP Server:

smtp.gmail.com


Advanced:
✔ Use TLS
Port: 587
Add Gmail Email & App Password

Click Test email.
