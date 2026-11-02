
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'docker build -t jenkins-cicd-project .'
            }
        }

        stage('Test') {
            steps {
                bat 'docker rm -f jenkins-test 2>nul || exit /b 0'
                bat 'docker run -d --name jenkins-test -p 8082:80 jenkins-cicd-project'
                bat 'timeout /t 5 /nobreak'
                bat 'curl.exe -f http://localhost:8082'
                bat 'docker rm -f jenkins-test'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker rm -f jenkins-cicd-project 2>nul || exit /b 0'
                bat 'docker run -d --restart unless-stopped --name jenkins-cicd-project -p 8083:80 jenkins-cicd-project'
            }
        }
    }
}
