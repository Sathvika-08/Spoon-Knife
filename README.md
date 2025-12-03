JENKINS
(tools)
To start jenkins(cmd as admin)
net start jenkins
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
install Copy Artifact plugins
for java project 
maven java build
maven java test

for web project
build 
test 
deploy

✔ Job 1 → MavenJava (build the Maven project)
✔ Job 2 → MavenJava_Test (copy artifacts & test)

✅ STEP 1: Create the First Freestyle Job (MavenJava)
1. Open Jenkins
http://localhost:8888

2. Create a New Job

Click New Item

Enter name: MavenJava

Select Freestyle project

Click OK

✅ STEP 2: Configure MavenJava
2.1 Add Description

Write something like:

This job builds the Maven Java project.

2.2 Source Code Management

Select Git

Enter your repository URL
Example:

https://github.com/yourname/maven-project.git

2.3 Build Steps
➤ Build Step 1: Clean

Click Add Build Step

Choose Invoke top-level Maven targets

Maven Version: MAVEN_HOME

Goals:

clean

➤ Build Step 2: Install

Click Add Build Step

Choose Invoke top-level Maven targets

Maven Version: MAVEN_HOME

Goals:

install

2.4 Post-Build Actions
➤ Action 1: Archive the Artifacts

Click Add post-build action

Select: Archive the artifacts

Files to archive:

**/*


This stores your built .jar and other files.

➤ Action 2: Build Other Projects

Click Add post-build action

Select: Build other projects

Projects to build:

MavenJava_Test


(This triggers the test job after build.)

2.5 Save Job

Click Apply → Save

🎉 MavenJava job is ready.

If you run it, you will see:

✔ Clean
✔ Install
✔ Artifacts archived
✔ Next project triggered

✅ STEP 3: Create the Second Freestyle Job (MavenJava_Test)
3.1 Create a New Job

Click New Item

Enter name: MavenJava_Test

Select Freestyle project

Click OK

✅ STEP 4: Configure MavenJava_Test
4.1 Description
This job copies artifacts from MavenJava and performs testing.

4.2 Source Code Management

Select None

4.3 Environment

Tick:
✔ Delete workspace before build starts

4.4 Build Steps
➤ Copy Artifacts from First Job

Click Add Build Step

Select Copy artifacts from another project

Project name:

MavenJava


Artifacts to copy:

**/*


This pulls the built .jar and files from job 1.

4.5 Post-Build Actions

Click Add post-build action

Choose Archive the artifacts

Files:

**/*

4.6 Save

Click Apply → Save

🎉 MavenJava_Test job is ready.

✅ STEP 5: Final Result
On Jenkins Dashboard:

You will see:

MavenJava

MavenJava_Test

✔ Running MavenJava:

It performs:

Git clone

Clean

Install

Archive artifacts

Triggers MavenJava_Test

You will see the console output like:

Build successful

Artifacts stored

✔ Running MavenJava_Test:

It performs:

Deletes workspace

Copies artifacts from MavenJava

Archives them again

🎉 Your Maven Java Automation System is now fully ready!


🌐 II. Maven Web Automation — Full Steps

You will create three Freestyle Jobs:

1️⃣ maven_web_build → Build the Maven Web App
2️⃣ maven_web_test → Copy artifacts + run tests
3️⃣ maven_web_deploy → Deploy .war to Tomcat

Then create a Pipeline View to visualize the automation.

--------------------------------------------
✅ 1. Create Freestyle Project: MavenWeb_Build
--------------------------------------------
Step 1: Open Jenkins
http://localhost:8888

Step 2: Create the Build Job

Click New Item

Enter name:

maven_web_build


Select Freestyle project

Click OK

✅ Step 3: Configure maven_web_build
3.1 Description
This job builds the Maven web application.

3.2 Source Code Management

Select Git

Enter your web project Git URL
(Example)

https://github.com/username/maven-webapp.git

3.3 Build Steps
➤ Build Step 1: Clean

Click Add Build Step

Choose Invoke top-level Maven targets

Maven version: MAVEN_HOME

Goals:

clean

➤ Build Step 2: Install

Click Add Build Step

Choose Invoke top-level Maven targets

Goals:

install

3.4 Post-Build Actions
➤ Action 1: Archive Artifacts

Click Add Post-build Action

Select Archive the artifacts

Files to archive:

**/*

➤ Action 2: Build Other Projects

Click Add Post-build Action

Select:

Build other projects


Projects to build:

maven_web_test

3.5 Save

Click Apply → Save

--------------------------------------------
✅ 2. Create Freestyle Project: MavenWeb_Test
--------------------------------------------
Step 1: Create Test Job

Click New Item

Name:

maven_web_test


Select Freestyle project

Click OK

✅ Step 2: Configure maven_web_test
2.1 Description
This job copies artifacts from the build job and executes tests.

2.2 Source Code Management

Select None

2.3 Environment

Check:
✔ Delete workspace before build starts

2.4 Build Steps
➤ Step 1: Copy Artifacts

Click Add Build Step

Select Copy artifacts from another project

Project name:

maven_web_build


Artifacts to copy:

**/*

➤ Step 2: Run Maven Tests

Click Add Build Step

Select Invoke top-level Maven targets

Maven version: MAVEN_HOME

Goals:

test

2.5 Post-Build Actions
➤ Action 1: Archive Artifacts

Click Add Post-build Action

Select Archive the artifacts

Files:

**/*

➤ Action 2: Build Other Projects

Click Add Post-build Action

Select:

Build other projects


Projects to build:

maven_web_deploy


Trigger condition:
✔ Trigger only if build is stable

2.6 Save

Click Apply → Save

--------------------------------------------
✅ 3. Create Freestyle Project: MavenWeb_Deploy
--------------------------------------------
Step 1: Create Deploy Job

Click New Item

Name:

maven_web_deploy


Select Freestyle project

Click OK

✅ Step 2: Configure maven_web_deploy
2.1 Description
This job deploys the generated WAR file to Tomcat.

2.2 Source Code Management

Select None

2.3 Environment

Check:
✔ Delete workspace before build starts

2.4 Build Steps
➤ Copy Artifacts

Click Add Build Step

Select Copy artifacts from another project

Project name:

maven_web_test


Artifacts to copy:

**/*

2.5 Post-Build Actions
➤ Deploy WAR to Tomcat

Click Add Post-build Action

Select:

Deploy war/ear to a container


WAR/EAR files:

**/*.war


Context path:

webapp


Tomcat URL example:

http://localhost:8080/manager/text


Give Tomcat credentials (configured in Jenkins)

2.6 Save

Click Apply → Save

--------------------------------------------
🎯 4. Create Pipeline View (MavenWeb Pipeline)
--------------------------------------------
Step 1: Create Pipeline View

On Jenkins dashboard click + next to “All”

Enter view name:

maven_web_pipeline


Select:
✔ Build Pipeline View

Click OK

Step 2: Configure View

Description (optional)

Upstream project:

maven_web_build


This automatically shows:

maven_web_build → maven_web_test → maven_web_deploy

Step 3: Save

Click Apply → Save

🎉 Final Output

Your Stage View will show a three-stage pipeline:

[ Build ] → [ Test ] → [ Deploy ]


Green = Successful
Red = Failed


java project

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

Small-web-project
pipeline {
    agent any

    stages {
        
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/ashi-06/small-webapp.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'No dependencies to install for static web project'
            }
        }

        stage('Build Web Project') {
            steps {
                echo 'No build step required for HTML/CSS/JS project'
            }
        }

        stage('Deploy to Local Server') {
            steps {
                echo 'Copying files to web server directory...'
                bat '''
                xcopy /E /Y . "C:\\inetpub\\wwwroot\\small-webapp\\"
                '''
            }
        }
    }
}

VIEW PIPELINE 
install Build Pipeline plugin in plugins to see build pipeline

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




2. Exploring git local and remote commands on the multi-folder project
git global configuration
git config --global user.name "ashi-06"
git config --global user.email "ashimahendra06@gmail.com"

Scenario-Based Git Commands
1. You've cloned a repository and made some changes to a local branch. Now you want to push these changes to the remote repository, but you're getting an error saying "rejected - non-fast-forward." How would you resolve this?

git pull --rebase origin <branch-name>

if there are no conflicts then
git push origin <branch-name>

If there are conflicts → Git will pause rebase
Step 1: Fix the conflict in the files

(Git marks them inside the file)

Step 2: Add the fixed file
git add <filename>

Step 3: Continue the rebase
git rebase --continue


Then push again:

git push origin <branch-name>


2. You’ve been working on a feature branch, and now you need to push it to the remote repository. However, the remote repository already has a main branch. How do you push your feature branch without affecting the main branch?

git push origin feature/feat-1

3. You cloned a remote repository, but after a while, the repository’s structure changed and new branches were added. How would you keep your local repository updated with the latest changes from the remote repository?

git fetch origin
brings all new branches and updates from GitHub into your local repository, without modifying your current work.

it checkout branch-name lets you switch to any new branch that was fetched

4. A colleague has pushed some changes to the main branch, but you have local changes in the same branch. You want to pull their changes, but you want to avoid merge conflicts. What steps would you take?
Use rebase to integrate their changes on top of your work:
git stash                   # Temporarily store your changes
git pull --rebase origin main
git stash pop               # Apply your changes on top

This reduces the chance of conflicts and keeps history clean.

5. You accidentally pushed a sensitive file (e.g., API keys) to the remote repository. How would you fix this situation?
Steps to remove the sensitive data:
    Remove the file and commit:
git rm --cached path/to/file
git commit -m "Remove sensitive file"
git push origin main
If the secret is in history, use git filter-branch or BFG Repo-Cleaner to rewrite history:
    git filter-branch --force --index-filter \
    "git rm --cached --ignore-unmatch path/to/file" \
    --prune-empty --tag-name-filter cat -- --all
    Force push and rotate the secret.

6. You’re working on a feature branch, and your manager requests that you integrate the latest changes from main into your feature branch. What steps would you take?
Use rebase or merge:
Rebase:
git checkout feature/your-feature
git fetch origin
git rebase origin/main

7. You cloned a remote repository, but later you find that you need to push your changes to a different remote repository. How do you configure your local repository to push to this new remote?
git remote set-url origin <new-remote-url>
git push origin branch-name

8. After running git pull, you notice that your local branch is behind the remote branch. How would you proceed to bring your local branch up to date without losing your local changes?
Use stash or rebase:
git stash
git pull --rebase origin branch-name
git stash pop
This ensures a clean rebase and retains your changes.

9. You’re working on a project with multiple collaborators, and you notice that your local changes conflict with changes that have been pushed by others. How would you resolve the conflicts?
    Pull the latest changes:
git pull origin branch-name
Git will highlight conflicts. Open the files, manually resolve the <<<<<<<, =======, and >>>>>>> markers.
Mark as resolved and commit:
    git add .
    git commit
git push origin branch-name

10. You’ve pushed a feature branch to a remote repository, but now you need to delete the branch from the remote. How would you do that?
Use the following command:
git push origin --delete feature/branch-name
This will remove the branch from the remote repository.


5. Docker CLI commands

docker --version
docker pull ubuntu:latest
docker run -it -p 9090:80 --name myubuntu1 ubuntu:latest
apt update
apt install nginx -y
service nginx start
ls
cd var
cd www
cd html
ls
rm index.nginx-debian.html
nano index.html
apt install nano
nano index.html
(type in index.html
<h1>Hello From Docker Ubuntu Nginx!</h1>)
visit http://localhost:9090
