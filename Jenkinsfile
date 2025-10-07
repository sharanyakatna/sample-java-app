pipeline {
    agent any

    tools {
        maven 'Maven 3.8.1'      // Make sure this Maven version exists in Jenkins
        git 'Default'            // Git installation name configured in Jenkins
    }

    environment {
        DOCKER_IMAGE = "sharanyakatna/sample-java-app:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sharanyakatna/sample-java-app.git',
                    credentialsId: '0189b9e1-87ca-4e52-83ae-605ba65d5ba0' // Your GitHub PAT credential ID
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-creds']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl set image deployment/sample-app-deployment sample-container=$DOCKER_IMAGE'
            }
        }
    }
}

