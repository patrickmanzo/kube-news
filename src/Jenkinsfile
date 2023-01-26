pipeline {
    agent any

    stages {

        stage ('Build Docker Image'){
            steps {
                script {
                    docker.app = docker.build("patrickmanzo/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')

                }
            }
        }
    }

}