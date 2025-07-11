pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/1sGu7/WebBanHang.git', branch: 'main'
            }
        }

        stage('Install Node & Run Test') {
            steps {
                sh '''
                    curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
                    apt-get install -y nodejs
                    npm install
                    npm test
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t my-image:latest .'
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
