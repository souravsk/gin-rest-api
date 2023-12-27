pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_TOKEN')
                ]) {
                    sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_TOKEN}"
                    sh "DOCKER_BUILDKIT=1 docker build -t sovu/gin-api -f cmd/server/Dockerfile ."
                    sh "docker push sovu/gin-api"
                }
            }
        }
    }

    post {
        success {
            script {
                sh 'docker logout'
            }
        }
    }
}
