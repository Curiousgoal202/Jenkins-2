pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Curiousgoal202/Jenkins-2.git'
            }
        }

        stage('Copy HTML to Server') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', 
                                                  keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        chmod 600 "$SSH_KEY"
                        scp -o StrictHostKeyChecking=no -i "$SSH_KEY" index.html ec2-user@18.144.54.98:/home/ec2-user/
                    '''
                }
            }
        }

        stage('Run HTML Server') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', 
                                                  keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no -i "$SSH_KEY" ec2-user@18.144.54.98 \
                        "nohup python3 -m http.server 8081 --directory /home/ec2-user/ > /dev/null 2>&1 &"
                    '''
                }
            }
        }
    }
}
