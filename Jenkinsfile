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
                sh '''
                    # Path where your PEM key is stored on Jenkins server
                    PEM_PATH="$HOME/.ssh/hdhd;.pem"

                    chmod 600 "$PEM_PATH"

                    scp -o StrictHostKeyChecking=no -i "$PEM_PATH" index.html ec2-user@18.144.54.98:/home/ec2-user/
                '''
            }
        }

        stage('Run HTML Server') {
            steps {
                sh '''
                    PEM_PATH="$HOME/.ssh/hdhd;.pem"

                    ssh -o StrictHostKeyChecking=no -i "$PEM_PATH" ec2-user@18.144.54.98 \
                    "nohup python3 -m http.server 8085 --directory /home/ec2-user/ > /dev/null 2>&1 &"
                '''
            }
        }
    }
}
