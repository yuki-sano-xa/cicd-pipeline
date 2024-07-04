pipeline {
    agent any

    environment {
        // Jenkinsの資格情報IDを設定
        DOCKERHUB_CREDENTIALS = credentials('docker_hub_creds_id')
    }

    stages {
        stage('Checkout') {
            steps {
                // GitHubリポジトリをチェックアウト
                git url: 'https://github.com/yuki-sano-xa/cicd-pipeline.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Dockerイメージをビルド
                script {
                    docker.build("yuki-sano-xa/cicd-pipeline:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                // Docker Hubにログインしてイメージをプッシュ
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_creds_id') {
                        docker.image("yuki-sano-xa/cicd-pipeline:${env.BUILD_NUMBER}").push()
                        docker.image("yuki-sano-xa/cicd-pipeline:${env.BUILD_NUMBER}").push("latest")
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
