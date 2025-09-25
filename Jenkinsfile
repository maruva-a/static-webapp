pipeline {
    agent any

    environment {
        // Replace this with the credential ID you added in Jenkins
        SSH_CREDENTIALS_ID = 'nginx-key'
        NGINX_USER = 'ubuntu'
        NGINX_HOST = '54.163.41.43'
        NGINX_WEBROOT = '/var/www/html/'
        TEMP_DIR = '/tmp/deploy/'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/maruva-a/static-webapp.git'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    sh """
                        echo "Deploying files to temporary directory on Nginx server..."
                        ssh -o StrictHostKeyChecking=no ${NGINX_USER}@${NGINX_HOST} "mkdir -p ${TEMP_DIR}"
                        scp -o StrictHostKeyChecking=no -r * ${NGINX_USER}@${NGINX_HOST}:${TEMP_DIR}
                        
                        echo "Moving files to Nginx webroot..."
                        ssh -o StrictHostKeyChecking=no ${NGINX_USER}@${NGINX_HOST} "sudo mv ${TEMP_DIR}* ${NGINX_WEBROOT}"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}

