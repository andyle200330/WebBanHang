pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/andyle200330/WebBanHang.git', branch: 'main'
            }
        }

        stage('Run HTML Test') {
            steps {
                sh '''
                    npm install
                    npm test
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-image:latest .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                    docker ps -q --filter "name=web_banhang" | xargs -r docker stop || true
                    docker ps -a -q --filter "name=web_banhang" | xargs -r docker rm || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8081:80 --name web_banhang my-image:latest'
            }
        }
    }
}
