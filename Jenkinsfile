pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/maruva-a/static-webapp.git'
            }
        }

        stage('Build') {
            steps {
                echo 'No build step needed for static HTML/CSS site'
            }
        }

        stage('Deploy to Nginx Server') {
            steps {
                sshagent(credentials: ['nginx-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@54.163.41.43 '
                        sudo rm -rf /var/www/html/* &&
                        exit
                        '
                        scp -r * ubuntu@<nginx-server-ip>:/tmp/deploy/
                        ssh -o StrictHostKeyChecking=no ubuntu@54.163.41.43 '
                        sudo cp -r /tmp/deploy/* /var/www/html/ &&
                        sudo systemctl restart nginx
                        '
                    '''
                }
            }
        }
    }
}
