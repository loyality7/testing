pipeline {
    agent any
    
    // Add trigger for GitHub webhook
    triggers {
        githubPush()
    }
    
    environment {
        DEPLOY_DIR = '/var/www/html'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Deploy') {
            steps {
                // Using sudo commands more safely
                sh '''
                    sudo rm -rf ${DEPLOY_DIR}/* || true
                    sudo cp -r ./* ${DEPLOY_DIR}/ || exit 1
                    sudo chown -R www-data:www-data ${DEPLOY_DIR}
                    sudo chmod -R 755 ${DEPLOY_DIR}
                '''
            }
        }
        
        stage('Verify') {
            steps {
                sh 'curl -f http://localhost || exit 1'
            }
        }
    }
    
    post {
        success {
            echo 'Website deployed successfully!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
