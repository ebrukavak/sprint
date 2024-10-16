pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yusufhkran/bcfm_academy_sprint_app.git'
            }
        }
        stage('Update Chart Version') {
            steps {
                sh 'helm version' // Helm sürümünü kontrol et 
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'aws eks --region us-east-1 update-kubeconfig --name sprint-cluster'
                sh 'cd academy2024-app-main-2/sprint-frontend-1.0.0/sprint-frontend'
                sh 'helm upgrade --install sprint-release sprint-frontend-1.0.0.tgz --values values.yaml'
            }
        }
    }
}
