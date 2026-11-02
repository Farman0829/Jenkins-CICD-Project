
pipeline {
    agent any

    environment {
        DOCKER = 'C:\\Users\\farma\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
    }

    stages {
        stage('Prepare') {
            steps {
                echo 'Creating Website Files'
                writeFile file: 'index.html', text: '''<!DOCTYPE html>
<html>
<head>
    <title>Jenkins CI/CD Demo</title>
</head>
<body>
    <h1>Welcome to Jenkins CI/CD</h1>
    <p>Application built and tested using Docker.</p>
</body>
</html>
'''
                writeFile file: 'Dockerfile', text: '''FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
'''
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker Image'
                bat '"%DOCKER%" build -t jenkins-demo .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Application'
                bat '"%DOCKER%" rm -f test-container 2>nul & ver >nul'
                bat '"%DOCKER%" run -d --name test-container -p 18081:80 jenkins-demo'
                bat 'ping -n 6 127.0.0.1 > nul'
                bat 'curl.exe --fail http://localhost:18081'
                bat '"%DOCKER%" rm -f test-container'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application'
                bat '"%DOCKER%" rm -f jenkins-deployed 2>nul & ver >nul'
                bat '"%DOCKER%" run -d --restart unless-stopped --name jenkins-deployed -p 18082:80 jenkins-demo'
            }
        }
    }
}
