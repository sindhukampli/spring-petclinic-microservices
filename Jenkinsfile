pipeline {
    agent any

    environment {
        ECR_REGISTRY = "654654623396.dkr.ecr.ap-south-1.amazonaws.com"
        AWS_REGION = "ap-south-1"
        HELM_CHART_PATH = "helm-charts/petclinic"
        IMAGE_NAME1 = "petclinic-service1"
        IMAGE_NAME2 = "petclinic-service2"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sindhukampli/spring-petclinic-microservices.git'
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    // AWS ECR login
                    sh 'aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}'

                    // Build Docker images
                    sh 'docker build -t ${ECR_REGISTRY}/${IMAGE_NAME1}:latest -f docker/Dockerfile-service1 .'
                    sh 'docker build -t ${ECR_REGISTRY}/${IMAGE_NAME2}:latest -f docker/Dockerfile-service2 .'

                    // Push Docker images to ECR
                    sh 'docker push ${ECR_REGISTRY}/${IMAGE_NAME1}:latest'
                    sh 'docker push ${ECR_REGISTRY}/${IMAGE_NAME2}:latest'
                }
            }
        }

        stage('Deploy with Helm') {
            steps {
                script {
                    sh "helm upgrade --install petclinic ${HELM_CHART_PATH} --values ${HELM_CHART_PATH}/values.yaml"
                }
            }
        }
    }
}
