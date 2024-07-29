pipeline {
    agent any

    environment {
        ECR_REGISTRY = "654654623396.dkr.ecr.ap-south-1.amazonaws.com"
        AWS_REGION = "ap-south-1"
        HELM_CHART_PATH = "helm-charts/petclinic"
        IMAGE_NAME1 = "petclinic-service1"
        IMAGE_NAME2 = "petclinic-service2"
        AWS_CREDENTIALS_ID = "terraform_user"  
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
                    // Check for credentials and login
                    withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: ' terraform_user ', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')])  {
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
        }

        stage('Deploy with Helm') {
            steps {
                script {
                    // Ensure Helm is installed and properly configured
                    sh "helm upgrade --install petclinic ${HELM_CHART_PATH} --values ${HELM_CHART_PATH}/values.yaml"
                }
            }
        }
    }
}
