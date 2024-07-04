pipeline {
    agent any

    environment {
        // Jenkins�̎��i���ID��ݒ�
        DOCKERHUB_CREDENTIALS = credentials('docker_hub_creds_id')
    }

    stages {
        stage('Checkout') {
            steps {
                // GitHub���|�W�g�����`�F�b�N�A�E�g
                git url: 'https://github.com/yuki-sano-xa/cicd-pipeline.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Docker�C���[�W���r���h
                script {
                    docker.build("yuki-sano-xa/cicd-pipeline:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                // Docker Hub�Ƀ��O�C�����ăC���[�W���v�b�V��
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
