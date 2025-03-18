pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/imakashpatil/node_hello.git'
        IMAGE_NAME = 'your-dockerhub-username/node-app'
        CONTAINER_NAME = 'node-app'
        APP_PORT = '3000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'docker_cred', passwordVariable: 'akashpatil.dev', usernameVariable: 'imakashpatil')]) {
                    sh 'echo $DOCKER_PASS | docker login -u your-dockerhub-username --password-stdin'
                    sh 'docker push ${IMAGE_NAME}'
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop ${CONTAINER_NAME} || true'
                sh 'docker rm ${CONTAINER_NAME} || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:${APP_PORT} ${IMAGE_NAME}'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful! 🎉'
        }
        failure {
            echo 'Deployment Failed! ❌'
        }
    }
}
