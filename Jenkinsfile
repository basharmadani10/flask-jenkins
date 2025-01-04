pipeline {
    agent any

    environment {
        USER_CREDENTIALS = credentials('dockerhub') // Assuming this is a Jenkins credential ID
    }
    
    stages {
        stage('Clone') {
            steps {
                // Clone the repository
                git branch: 'main', url: 'https://github.com/basharmadani10/flask-jenkins.git'
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
                sh "pytest"
            }
        }
        stage('Dockerize') {
            steps {
                script {
                    // Extract username and password from the credentials
                    def user = USER_CREDENTIALS.username
                    def password = USER_CREDENTIALS.password

                    // Log in to Docker Hub
                    sh '''
                    echo ${password} | docker login -u ${user} --password-stdin
                    '''
                    
                    // Build the Docker image (make sure you have a Dockerfile in the repo)
                    sh '''
                    docker build -t mo1074/flask-app:v1 .
                    '''
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    sh '''
                    docker push mo1074/flask-app:v1
                    '''
                }
            }
        }
    }
}
