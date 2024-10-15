pipeline {
  agent any
  environment {
    ECR_REGISTRY="571600829776.dkr.ecr.us-east-1.amazonaws.com/sprint"
  }

  stages {
    stage('Build App Docker Images') {
      steps {
        echo 'Building App Dev Images'
        sh "docker build -t \"${ECR_REGISTRY}/:sprint-b${BUILD_NUMBER}\" ./"  
        sh 'docker image ls'
      }
    }
    stage('Push Images to ECR Repo') {
      steps {
        echo "Pushing App Images to ECR Repo"
        sh 'aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}'
        sh "docker push ${ECR_REGISTRY}/:sprint-b${BUILD_NUMBER}"
      }
    }
  }
}
