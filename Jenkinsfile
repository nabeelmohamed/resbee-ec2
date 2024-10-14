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
                script {
                    // Replace with your actual paths and user details
                    def remoteUser = 'ec2-user'
                    def remoteHost = '54.174.151.2'
                    def sshKey = '/home/nabeel/ec2-user.pem'
                    
                    // SSH and SCP Commands
                    sh """
                    ssh -i ${sshKey} -o StrictHostKeyChecking=no ${remoteUser}@${remoteHost} 'mkdir -p /home/nabeel/ec2-project/resbee-ec2/mysite'
                    scp -i ${sshKey} -o StrictHostKeyChecking=no index.php ${remoteUser}@${remoteHost}:/home/nabeel/ec2-project/resbee-ec2/mysite/
                    """
                    
                    // Restart Nginx on the EC2 instance to apply changes
                    sh """
                    ssh -i ${sshKey} -o StrictHostKeyChecking=no ${remoteUser}@${remoteHost} 'sudo systemctl restart nginx'
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
