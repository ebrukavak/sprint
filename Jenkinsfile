pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yusufhkran/bcfm_academy_sprint_app.git'
            }
        }
        stage('Pull Image from ECR') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 571600829776.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker pull 571600829776.dkr.ecr.us-east-1.amazonaws.com/sprint-frontend:sprint-frontend:latest'
            }
        }
        stage('Create Helm Chart') {
            steps {
                sh 'helm create my-chart'
                sh 'cp templates/* charts/my-chart/templates/'
                sh 'sed -i "s/image: .*/image: 571600829776.dkr.ecr.us-east-1.amazonaws.com\/spring-frontend:latest/" charts/my-chart/templates/deployment.yaml'
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'helm upgrade --install my-release charts/my-chart'
            }
        }
    }
}
