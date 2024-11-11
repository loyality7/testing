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
            echo "=== Workspace Files ==="
            ls -la /var/lib/jenkins/workspace/test
            echo "===================="
            
            echo "=== Copying Files ==="
            sudo cp -rv /var/lib/jenkins/workspace/test/* /var/www/html/ || echo "Copy failed"
            echo "=== Files in /var/www/html ==="
            ls -la /var/www/html/
            echo "===================="
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
