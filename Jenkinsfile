pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/1sGu7/WebBanHang.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'DOCKER_BUILDKIT=1 docker build -t my-image:latest .'
                }
            }
        }
        stage('Stop Existing Container') {
            steps {
                script {
                    sh '''
                        docker ps -q --filter "name=web_banhang" | xargs -r docker stop || true
                        docker ps -a -q --filter "name=web_banhang" | xargs -r docker rm || true
                    '''
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -d -p 8081:80 --name web_banhang my-image:latest'
                }
            }
        }
    }
}