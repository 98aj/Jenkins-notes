# Jenkins-notes

Link:https://www.jenkins.io/doc/book/installing/linux/

Step:1 Update the System EC2 instance
------------------------------
> sudo apt-get update

Step:2 Install java on EC2 instance
------------------------------
> sudo apt install openjdk-17-jdk -y

Step:3 Check the java version
------------------------------
> java --version

Step:4 Jenkins Installtion on EC2 instance
-------------------------------
# Add GPG key
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add Jenkins repo using the correct keyring reference
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y

Step:5 start Jenkins service
-----------------------------------
a. enable Jenkins
> sudo systemctl enable jenkins

b. start the jenkins
> sudo systemctl start jenkins

c. stop jenkins service
> sudo systemctl stop jenkins
> now jenkins server will run on localhost:8080 on EC2 insance
> To access this server 

Step 6: Open Jenkins port in EC2 security group
Go to your AWS Console → EC2 → Security Groups.

Edit inbound rules of the group attached to your EC2 instance.

Add a rule:

Type: Custom TCP

Port: 8080

Source: Anywhere (or your IP only for security)

now access jenkins in you browser 

http://<YOUR_EC2_PUBLIC_IP>:8080


Step: 7 Get Secret Inital Admin Password
> sudo cat /var/lib/Jenkins/secrets/initialAdminPassword

Step:8 Click on Save and Continue

Step:9 Install required plugins


*******************************************************
EXERCISE:1
*******************************************************
> goto> jenkins>dashboard>create new job>
select> free style project> give the name >give the description
> goto> scource code management>
    GIT: select
    URL: your github repository URL
    Branches to build: */master or /main
    POLL SCM:
        H/2 * * * *
> click on apply and Save
> build the project


>now goto> you github repository and commit some changes , once a new change in your github repositoryis detected
jenkins will automatically build the project and give you the result

that same can be check in jenkins console output as well

Note:
 meaning: H/2 means----> H represents some hash, 2 represents 2 minutes
            *----------> Hours
            *----------> Day
            *----------> Month
            *----------> Every Day of Week
******************************************************************
Exercise:2 Integrating Plugins to the Jenkins
******************************************************************
> goto> manage Jenkins > Plugins> Available Plugins> Install Maven Integration> Install

Create a Simple Pipeline Project
------------------------------
> new item> select Pipeline Project> give the name > write your own Pipeline

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'This is Checkout Stage'
            }
        }
        stage('Build') {
            steps {
                echo 'This is Build Stage'
            }
        }
        stage('Deployment') {
            steps {
                echo 'This is Deployment Stage'
            }
        }
    }
}


Save and apply
> build the pipline and check the console output

******************************************************************
Exercise:3 create a pipeline to check java version
******************************************************************
pipeline {
    agent any

    stages {
        stage('Check Java Version') {
            steps {
                echo 'Check Java Version'

                sh 'java --version'

            }
        }

    }
}

Task: Trigger Jenkins Build on GitHub Push

 1. Install Required Jenkins Plugins
✅ Git Plugin

✅ GitHub Plugin

✅ GitHub Integration

✅ GitHub Branch Source (optional for multibranch pipelines)

Install from: Manage Jenkins → Manage Plugins

 2. Configure Jenkins Job
In your Jenkins job (Freestyle or Pipeline):

Source Code Management → Git:
Set the repo URL and credentials (if private)

Build Triggers:
✅ Check GitHub hook trigger for GITScm polling

3. Create a GitHub Webhook
In your GitHub repo:

Go to Settings → Webhooks → Add webhook

Payload URL: http://<your-jenkins-ip>:8080/github-webhook/

Content Type: application/json

Event: Select “Just the push event”

Click Add webhook

4. Verify Webhook and Test Push
Push code to GitHub

GitHub → Webhook should return 200 OK

Jenkins should auto-trigger a build

Check Jenkins Build History → Console Output

