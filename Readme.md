CI/CD Pipeline for Node.js App using Jenkins and AWS EC2
This project demonstrates a complete CI/CD pipeline setup for a simple Node.js application using Jenkins. The pipeline automates code checkout, dependency installation, application run, and basic validation. The deployment is done on an AWS EC2 instance.

Tech Stack
Jenkins – For building the CI/CD pipeline

Git & GitHub – Version control and SCM

AWS EC2 (Ubuntu) – Hosting Jenkins and the app

Node.js & npm – Application environment

Shell Scripting – Running commands and scripts

VS Code – Development environment

Project Structure
bash
Copy
Edit
.
├── Jenkinsfile           # Pipeline configuration
├── package.json
├── app.js / index.js     # Your Node.js app entry point
└── node_modules/
CI/CD Pipeline Flow
Jenkinsfile
pipeline {
    agent any

    tools {
        nodejs "nodejs"  // Defined in Jenkins tool config
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run App') {
            steps {
                sh 'nohup npm start &'
            }
        }
    }

    post {
        success {
            echo "CI/CD pipeline executed successfully!"
        }
        failure {
            echo "CI/CD pipeline failed."
        }
    }
}
Step-by-Step Setup
1. Setup AWS EC2 Instance
Launch a Ubuntu EC2 instance.

Allow ports 22, 8080 (Jenkins), and 3000 (Node app).

SSH into the server and update it:

sudo apt update && sudo apt upgrade
2. Install Jenkins
Install Java and Jenkins


sudo apt install openjdk-17-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins
Access Jenkins via http://<your-ec2-public-ip>:8080

Unlock Jenkins and install recommended plugins.

3. Install Node.js

curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
4. Configure Jenkins
Install NodeJS plugin in Jenkins

Go to Manage Jenkins > Global Tool Configuration

Add NodeJS version (e.g. nodejs)

5. Create Jenkins Job
Create a Pipeline project

Connect your GitHub repository (SCM)

Paste the Jenkinsfile content
pipeline {
    agent any

    tools {
        nodejs "nodejs"  // Make sure 'nodejs' is defined in Jenkins Global Tool Config
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                sh 'ls -la'  // For debugging
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run App') {  
            steps {
        sh '''
            nohup npm start > app.log 2>&1 &
        '''
            }
        }
}

    post {
        success {
            echo "✅ CI/CD pipeline executed successfully!"
        }
        failure {
            echo "❌ CI/CD pipeline failed."
        }
    }
}


Click Build Now

🧪 Common Errors & Fixes
Issue	Fix
Jenkins Build not finishing (red line)	Used nohup npm start & to run app in background
server.js not found	Ensured app runs with npm start referencing the correct main file
Jenkins not detecting Node.js	Configured NodeJS tool in Jenkins
Access denied to Git repo	Made GitHub repo public or configured SSH keys
Jenkins not running	Checked sudo systemctl status jenkins and opened port 8080