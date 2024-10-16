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
                sh 'helm dependency update' // Chart bağımlılıklarını güncelle
                sh 'helm chart version sprint-frontend-1.0.0.tgz' 
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'helm upgrade --install sprint-release sprint-frontend-1.0.0.tgz --values values.yaml'
            }
        }
    }
}
