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
                // Assuming AWS credentials are stored securely (replace with your method)
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 571600829776.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker pull 571600829776.dkr.ecr.us-east-1.amazonaws.com/sprint-frontend:sprint-frontend:latest'
            }
        }
        stage('Create Helm Chart') {
            steps {
                sh 'helm create my-chart'
                sh 'cp templates/* charts/my-chart/templates/'
                // Consider using a values.yaml file for flexibility
                sh 'sed -i "s/image: .*/image: {{ .image }}/" charts/my-chart/templates/deployment.yaml'
            }
        }
        stage('Deploy to EKS') {
            steps {
                // Assuming a values.yaml file exists (modify if not)
                sh 'helm upgrade --install my-release charts/my-chart -f values.yaml'
            }
        }
    }
}
