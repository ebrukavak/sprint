pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yusufhkran/bcfm_academy_sprint_app.git'
            }
        }

        stage('Create Helm Chart') {
            steps {
                sh 'helm create my-chart'
                sh 'rm -rf my-chart/templates/*'
                sh 'cp -r academy2024-app-main-2/frontend/deployment.yml my-chart/templates/'
            }
        }

        stage('Package Chart') {
            steps {
                sh 'helm package my-chart'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'helm install my-release my-chart-0.1.0.tgz'
            }
        }
    }
}
