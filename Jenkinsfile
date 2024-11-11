pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Debug') {
            steps {
                sh '''
                    echo "Current workspace:"
                    pwd
                    echo "Files in workspace:"
                    ls -la
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                    echo "Starting deployment..."
                    sudo mkdir -p /var/www/html
                    sudo rm -rf /var/www/html/*
                    sudo cp -r * /var/www/html/
                    echo "Files copied to /var/www/html:"
                    ls -la /var/www/html/
                '''
            }
        }
    }
}
