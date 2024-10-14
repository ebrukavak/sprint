pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t 571600829776.dkr.ecr.region.amazonaws.com/sprint:latest .'
            }
        }
        stage('Push') {
            steps {
                script {
                   
                    def registryId = '571600829776'
                    def region = 'us-east-1'
                    def repositoryName = 'sprint'
                    def imageName = "$registryId.dkr.ecr.$region.amazonaws.com/$repositoryName:latest"

                    // Eğer repo daha önce oluşturulmamışsa oluştur
                    sh "aws ecr create-repository --repository-name $repositoryName --region $region"

                    def command = "aws ecr get-login-password --region $region | docker login --username AWS --password-stdin $registryId.dkr.ecr.$region.amazonaws.com"
                    sh command
                    sh "docker push $imageName"
                }
            }
        }
    }
}
