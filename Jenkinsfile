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

        // CD - Continuous Delivery
        stage ('Deploy Kubernetes') {
           environment {
               tag_version = "${env.BUILD_ID}"
            }
           steps {
               withKubeConfig ([credentialsId: 'kubeconfig']) {
                   sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                   sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }
}