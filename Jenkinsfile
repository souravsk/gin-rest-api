pipeline {
    agent any

    environment {
        GITHUB_CREDENTIALS = credentials('github')
        DOCKERHUB_USERNAME_CREDENTIALS = credentials('DOCKERHUB_USERNAME')
        DOCKERHUB_TOKEN_CREDENTIALS = credentials('DOCKERHUB_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/cd-pipeline']],
                        userRemoteConfigs: [[url: 'https://github.com/souravsk/gin-rest-api.git/']],
                        credentialsId: GITHUB_CREDENTIALS
                    ])
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    withCredentials([
                        usernamePassword(credentialsId: DOCKERHUB_TOKEN_CREDENTIALS, usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_TOKEN')
                    ]) {
                        sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_TOKEN_CREDENTIALS}"
                        // Your build and push steps using DOCKERHUB_USERNAME and DOCKERHUB_TOKEN_CREDENTIALS
                    }
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
