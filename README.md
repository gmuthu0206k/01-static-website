🚀 END-TO-END CI/CD PIPELINE WITH JENKINS & DOCKER
Automated Static Website Deployment on AWS EC2

============================================================

📌 PROJECT SUMMARY

A hands-on DevOps project demonstrating automation of code integration,
containerization, image publishing, and deployment using Jenkins, Docker,
GitHub, and AWS.

This project automatically deploys a static website whenever code is pushed
to GitHub.

============================================================

🏗️ ARCHITECTURE FLOW

Developer Push Code
      ↓
GitHub Repository
      ↓
Jenkins Pipeline
      ↓
Docker Build Image
      ↓
Push to Docker Hub
      ↓
Deploy on AWS EC2
      ↓
Website Live via Nginx

============================================================

🧰 TECH STACK

- Jenkins       : CI/CD Automation
- Docker        : Containerization
- Docker Hub    : Image Registry
- GitHub        : Source Code Management
- AWS EC2       : Deployment Server
- Ubuntu Linux  : Host OS
- Nginx         : Web Server

============================================================

📁 REPOSITORY STRUCTURE

.
├── index.html
├── Dockerfile
├── Jenkinsfile
└── README.md

============================================================

⚙️ CI/CD PIPELINE STAGES

1. Source Code Checkout
   Jenkins pulls latest code from GitHub.

2. Build Docker Image
   Creates lightweight Nginx image for static site.

3. Docker Hub Login
   Uses secure Jenkins credentials.

4. Push Docker Image
   Uploads versioned image to Docker Hub.

5. Deploy Latest Container
   Removes old container and runs latest version.

============================================================

🐳 DOCKERFILE

FROM nginx:alpine

RUN rm -rf /usr/share/nginx/html/*

COPY . /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

============================================================

🔁 JENKINSFILE

pipeline {
    agent any

    environment {
        IMAGE_NAME = "your-dockerhub-username/static-site"
        IMAGE_TAG  = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {
                    sh '''
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Deploy Latest Container') {
            steps {
                sh '''
                    docker rm -f static-site || true
                    docker run -d -p 8081:80 --name static-site $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

============================================================

☁️ AWS EC2 SETUP

Instance Type : t3.medium
OS            : Ubuntu 22.04 / 24.04

Open Ports:
- 22   : SSH
- 8080 : Jenkins
- 8081 : Website

============================================================

🌐 LIVE APPLICATION

http://<EC2-PUBLIC-IP>:8081

============================================================

📈 KEY DEVOPS SKILLS DEMONSTRATED

✔ CI/CD Pipeline Design
✔ Jenkins Automation
✔ Docker Image Lifecycle
✔ Secure Credential Management
✔ AWS EC2 Deployment
✔ Linux Server Administration
✔ Deployment Troubleshooting

============================================================

🚀 FUTURE ENHANCEMENTS

- Kubernetes Deployment
- SonarQube Integration
- Trivy Security Scan
- Prometheus + Grafana Monitoring
- Terraform Infrastructure as Code
- Blue/Green Deployment

============================================================

💼 WHY THIS PROJECT MATTERS

This project reflects real-world DevOps responsibilities:

- Automating deployments
- Reducing manual effort
- Faster software delivery
- Scalable container deployment
- Reliable CI/CD workflow

============================================================

👨‍💻 AUTHOR

Mutharasan Gopalakrishnan
Aspiring DevOps Engineer

Skills:
AWS | Jenkins | Docker | Linux | GitHub | CI/CD

============================================================
