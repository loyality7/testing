pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Debug Workspace') {
            steps {
                sh '''
                    echo "=== Workspace Info ==="
                    pwd
                    ls -la
                    echo "===================="
                '''
            }
        }
        
        stage('Prepare Apache') {
            steps {
                sh '''
                    echo "=== Preparing Apache ==="
                    sudo systemctl status apache2
                    sudo rm -rf /var/www/html/*
                    echo "===================="
                '''
            }
        }
        
        stage('Deploy Files') {
            steps {
                sh '''
                    echo "=== Deploying Files ==="
                    sudo cp -rv * /var/www/html/ || echo "Copy failed"
                    echo "=== Deployed Files ==="
                    ls -la /var/www/html/
                    echo "===================="
                '''
            }
        }
        
        stage('Set Permissions') {
            steps {
                sh '''
                    echo "=== Setting Permissions ==="
                    sudo chown -R www-data:www-data /var/www/html/
                    sudo chmod -R 755 /var/www/html/
                    ls -la /var/www/html/
                    echo "===================="
                '''
            }
        }
        
        stage('Verify Apache') {
            steps {
                sh '''
                    echo "=== Apache Status ==="
                    sudo systemctl status apache2
                    echo "=== Apache Config ==="
                    sudo apache2ctl -S
                    echo "===================="
                '''
            }
        }
    }
    
    post {
        always {
            sh '''
                echo "=== Final Directory Status ==="
                ls -la /var/www/html/
                echo "=== Apache Error Log ==="
                sudo tail -n 20 /var/log/apache2/error.log
                echo "===================="
            '''
        }
    }
}
