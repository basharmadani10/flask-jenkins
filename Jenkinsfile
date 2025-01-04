pipeline {
    agent any

    environment {
        USER_CREDENTIALS = credentials('dockerhub') // Assuming this is a Jenkins credential ID
    }
    
    stages {
        stage('Clone') {
            steps {
                // Clone the repository
                git branch: 'master', url: 'https://github.com/basharmadani10/flask-jenkins.git'
            }
        }
        stage('Setup') {
            steps {
                // Install dependencies
                sh "pip install -r requirements.txt"
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh " python3 -m pytest"
            }
        }
        stage('Dockerize') {
            steps {
                script {
                    // Build the Docker image (make sure you have a Dockerfile in the repo)
                    sh '''
                    docker build -t mo1074/flask-app:v1 .
                    '''
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                // Use withCredentials to access Docker Hub credentials
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Log in to Docker Hub
                    sh '''
                    echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                    '''
                    
                    // Push the Docker image to Docker Hub
                    sh '''
                    docker push mo1074/flask-app:v1
                    '''
                }
            }
        }
    }
}
