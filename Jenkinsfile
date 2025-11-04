pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "rushibhor78/frontend"
        DOCKER_TAG = "v1"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'develop', url: 'https://github.com/rushibh0r/microservices-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dir('src/frontend') {
                        sh 'docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .'
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKERHUB_PASS')]) {
                    sh '''
                    echo $DOCKERHUB_PASS | docker login -u rushibhor78 --password-stdin
                    docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    '''
                }
            }
        }

        stage('Post-Build') {
            steps {
                echo '✅ Frontend Docker Image built and pushed successfully!'
            }
        }
    }
}
