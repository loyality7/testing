pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('HTML Validation') {
            steps {
                sh '''
                    echo "Checking HTML format..."
                    if grep -r "<!DOCTYPE html>" ./*.html; then
                        echo "HTML validation passed"
                    else
                        echo "HTML validation failed"
                        exit 1
                    fi
                '''
            }
        }
        
        stage('Deploy to Apache') {
            steps {
                script {
                    // Assuming Apache is installed and configured
                    sh '''
                        sudo rm -rf /var/www/html/*
                        sudo cp -r * /var/www/html/
                        sudo chown -R www-data:www-data /var/www/html
                    '''
                }
            }
        }
        
        stage('Test Deployment') {
            steps {
                sh 'curl -f http://localhost || exit 1'
            }
        }
    }
}
