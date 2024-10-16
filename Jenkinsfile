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
                sh 'helm package my-chart -d .'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'aws eks --region us-east-1 update-kubeconfig --name sprint-cluster'
                sh 'cd .. && cd ..' 
                sh 'kubectl label deployment sprint-frontend app.kubernetes.io/managed-by=Helm'
                sh 'kubectl annotate deployment sprint-frontend meta.helm.sh/release-name=my-release'
                sh 'kubectl annotate deployment sprint-frontend meta.helm.sh/release-namespace=default'
                sh 'helm install my-release ./my-chart-0.2.0.tgz'
            }
        }

    }
}
