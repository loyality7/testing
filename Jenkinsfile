pipeline {
    agent any
    
    environment {
        DEPLOY_DIR = '/var/www/html'  // Default Nginx directory
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Deploy') {
            steps {
                // Simple copy of website files to Nginx directory
                sh '''
                    sudo rm -rf ${DEPLOY_DIR}/*
                    sudo cp -r ./* ${DEPLOY_DIR}/
                    sudo chown -R www-data:www-data ${DEPLOY_DIR}
                    sudo chmod -R 755 ${DEPLOY_DIR}
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Website deployed successfully!'
        }
    }
}
