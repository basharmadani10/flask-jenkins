pipeline {
    agent any

    environment {
        USER_CREDENTIALS = credentials('dockerhub') // Jenkins credentials ID
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
                sh "python3 -m pytest"
            }
        }
        stage('Dockerize') {
            steps {
                script {
                    // Build the Docker image (ensure a Dockerfile is present in the repo)
                    sh '''
                    docker build -t mo1074/flask-app:v1 .
                    '''
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Log in to Docker Hub and push the image
                    sh '''
                    echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                    docker push mo1074/flask-app:v1
                    '''
                }
            }
        }
        stage('Deployment') {
            steps {
                // Apply Kubernetes manifests
                sh '''
                kubectl apply -f all.yml
                '''
            }
        }
    }
}
