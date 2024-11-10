pipeline {
    agent any
    
    triggers {
        githubPush()
    }
    
    stages {
        stage('Debug Info') {
            steps {
                sh '''
                    echo "=== Workspace Info ==="
                    pwd
                    ls -la
                    
                    echo "=== Git Info ==="
                    git status
                    git remote -v
                    git rev-parse HEAD
                '''
            }
        }
        
        stage('Clean Workspace') {
            steps {
                cleanWs()
                checkout scm
            }
        }
        
        stage('Verify Files') {
            steps {
                sh '''
                    echo "=== Files to Deploy ==="
                    ls -la
                    cat index.html
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                    echo "=== Deploying Files ==="
                    # Remove old files
                    sudo rm -rf /var/www/html/*
                    
                    # Copy new files
                    sudo cp -rv ./* /var/www/html/
                    
                    # Set permissions
                    sudo chown -R www-data:www-data /var/www/html
                    sudo chmod -R 755 /var/www/html
                    sudo find /var/www/html -type f -exec chmod 644 {} \\;
                    
                    echo "=== Deployed Files ==="
                    ls -la /var/www/html/
                '''
            }
        }
    }
    
    post {
        always {
            sh '''
                echo "=== Final Deployment State ==="
                ls -la /var/www/html/
                echo "=== Nginx Status ==="
                sudo systemctl status nginx
                echo "=== Latest Git Commit ==="
                git log -1
            '''
        }
    }
}
