pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/nabeelmohamed/resbee-ec2.git', branch: '${env.BRANCH_NAME}'
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['ssh-ec2-key']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@54.174.151.2 'mkdir -p /home/nabeel/ec2-project/resbee-ec2/mysite'
                    scp -o StrictHostKeyChecking=no index.php ec2-user@54.174.151.2:/home/nabeel/ec2-project/resbee-ec2/mysite/
                    sudo systemctl restart nginx
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

