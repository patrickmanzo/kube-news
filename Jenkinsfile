pipeline {
    agent any

    stages {
        // CI - Continuous Integration
        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("patrickmanzo/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com/', 'dockerhubid') {
                        dockerapp.push("${env.BUILD_ID}")
                }
            }
        }
    }

}