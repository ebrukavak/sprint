pipeline {
    agent any
    parameters {
        string(name: 'CHART_PATH', defaultValue: 'my-chart', description: 'Chart klasörü adı')
        string(name: 'CHART_VERSION', defaultValue: 'v1.0.0', description: 'Chart versiyonu')
        string(name: 'WORKSPACE', defaultValue: '/var/lib/jenkins/workspace/your_job_name', description: 'Jenkins workspace yolu')
    } 
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
                sh "helm install my-release ${params.WORKSPACE}/${params.CHART_PATH}-${params.CHART_VERSION}.tgz"
            }
        }
    }
}
