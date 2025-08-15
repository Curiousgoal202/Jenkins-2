pipeline {
    agent any

    environment {
        SERVER_USER = "ec2-user"               // change to your username
        SERVER_IP = "18.144.54.98"             // change to your server IP
        SSH_KEY = credentials('server-ssh-key') // Jenkins SSH key credentials
        CUSTOM_PORT = "8085"                    // change to your desired port
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Curiousgoal202/Jenkins-2.git'
            }
        }

stage('Copy HTML to Server') {
    steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'SSH_KEY')]) {
            sh '''
                scp -o StrictHostKeyChecking=no -i "$SSH_KEY" index.html ec2-user@18.144.54.98:/home/ec2-user/
            '''
        }
    }
}

        stage('Run HTML Server') {
            steps {
                sh """
                ssh -i $SSH_KEY $SERVER_USER@$SERVER_IP '
                    nohup python3 -m http.server $CUSTOM_PORT --directory /home/$SERVER_USER/ > /dev/null 2>&1 &
                '
                """
            }
        }
    }

    post {
        success {
            echo "Your HTML file is now available at: http://$SERVER_IP:$CUSTOM_PORT"
        }
    }
}
