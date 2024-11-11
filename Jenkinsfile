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
        
         stage('Deploy Files') {
    steps {
        sh '''
            echo "=== Copying Files ==="
            sudo cp -r /var/lib/jenkins/workspace/test/* /var/www/html/
            sudo chown -R www-data:www-data /var/www/html/
            sudo chmod -R 755 /var/www/html/
            sudo systemctl restart apache2
            echo "=== Apache Service Status ==="
            sudo systemctl status apache2
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
