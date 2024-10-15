'''groovy
pipeline {
  agent any
  environment {
    APP_NAME="sprint"
    APP_REPO_NAME="A-repo/${APP_NAME}-frontend"
    AWS_ACCOUNT_ID=sh(script:'aws sts get-caller-identity --query Account --output text', returnStdout:true).trim()
    AWS_REGION="us-east-1"
    ECR_REGISTRY="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"

    stages {
      stage('Create ECR Repo') {
        steps {
          echo "Creating ECR Repo for ${APP_NAME} app"
          sh '''

                aws ecr create-repository \
                  --repository-name ${APP_REPO_NAME} \
                  --image-scanning-configuration scanOnPush=true \
                  --image-tag-mutability MUTABLE \
                  --region ${AWS_REGION}   

          '''  # Added the missing closing parenthesis
        }
      }
      stage('Build App Docker Images') {
        steps {
          echo 'Building App Dev Images'
          sh "docker build -t \"${ECR_REGISTRY}/${APP_REPO_NAME}:sprint-b${BUILD_NUMBER}\" ./"  # Use double quotes for variable interpolation
          sh 'docker image ls'
        }
      }
      stage('Push Images to ECR Repo') {
        steps {
          echo "Pushing ${APP_NAME} App Images to ECR Repo"
          sh 'aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}'   

          sh "docker push ${ECR_REGISTRY}/${APP_REPO_NAME}:sprint-b${BUILD_NUMBER}"   

        }
      }
    }
  }
}
