pipeline {
    agent any

    environment {
        USER_CREDENTIALS = credentials('username-password') // Assuming this is a Jenkins credential ID
    }
    
    stages {
     
        
        stage('Setup') {
            steps {
                sh "pip install -r requirements.txt"
                sh " pip install pytest"
            }
        }
        stage('Test') {
            steps {
              sh 'python3 -m pytest'
            }
        }
        stage('Package code') {
            steps {
                sh "zip -r myapp.zip ./* -x '*.git*'"
                sh "ls -lart"
            }
        }
        stage('Deploy to Prod') {
            steps {
                script {
                    // Extract username and password from the credentials
                    def user = USER_CREDENTIALS.username
                    def password = USER_CREDENTIALS.password

                    // Use sshpass to deploy the application
                    sh """

                        unzip -o /home/bm10/myapp.zip -d /home/bm10/app/
                        source /home/bm10/app/venv/bin/activate
                        cd /home/bm10/app/
                        pip install -r requirements.txt
                        sudo systemctl restart flaskapp.service
EOF
                    """
                }
            }
        }
    }
}
