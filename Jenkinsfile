pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '${env.BRANCH_NAME}']],
                    userRemoteConfigs: [[url: 'https://github.com/nabeelmohamed/resbee-ec2.git', credentialsId: '1f5d1ce7-6d17-4259-a348-b2bcc947a292']]
                ])
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['ssh-ec2-key']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@54.174.151.2 'mkdir -p /home/nabeel/ec2-project/resbee-ec2/mysite'
                    scp -o StrictHostKeyChecking=no index.php ec2-user@54.174.151.2:/home/nabeel/ec2-project/resbee-ec2/mysite/
                    ssh -o StrictHostKeyChecking=no ec2-user@54.174.151.2 'sudo systemctl restart nginx'
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
