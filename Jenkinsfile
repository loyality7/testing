pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Clean workspace before checkout
                cleanWs()
                checkout scm
            }
        }
        
        stage('List Files') {
            steps {
                // Debug: List files in workspace
                sh 'pwd'
                sh 'ls -la'
            }
        }
        
        stage('Deploy to Apache') {
            steps {
                script {
                    // Debug: Print current location
                    sh '''
                        echo "Current directory contents:"
                        ls -la
                        
                        echo "Cleaning up old files..."
                        sudo rm -rf /var/www/html/*
                        
                        echo "Copying new files..."
                        sudo cp -r index.html css js /var/www/html/
                        
                        echo "Setting permissions..."
                        sudo chown -R www-data:www-data /var/www/html/
                        sudo chmod -R 755 /var/www/html/
                        
                        echo "Verifying deployed files:"
                        ls -la /var/www/html/
                    '''
                }
            }
        }
        
        stage('Verify Deployment') {
            steps {
                sh '''
                    echo "Checking deployed files:"
                    ls -la /var/www/html/
                    
                    echo "Testing web server:"
                    curl -I http://localhost
                '''
            }
        }
    }
}
