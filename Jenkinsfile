pipeline {
  agent any
  tools {
    maven 'Maven_3_5_2'
  }
  environment {
    AWS_ACCOUNT_ID="381050469176"
    AWS_DEFAULT_REGION="eu-north-1"
    IMAGE_REPO_NAME="innoqadevsecops"
    IMAGE_TAG="latest"
    REPOSITORY_URI = "381050469176.dkr.ecr.eu-north-1.amazonaws.com/innoqadevsecops"
  }
  stages {
    stage('Compile and Run Sonar Analysis') {
      steps {
        sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=alfonso84_testing_devsecops -Dsonar.organization=alfonso84 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=b91e53be856225ba01195f41378b2292b6e93a11'
      }
    }
  }

  stages {
    stage('Login to AWS ECR') {
      steps {
        script {
          sh '''aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com'''
        }
      }
    }
  }

  stages {
    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_REPO_NAME}:${IMAGE_TAG}")
        }
      }
    }
  }

  stages {
    stage('Tag image') {
      steps {
        script {
          dockerImage.withTag(REPOSITORY_URI + ":" + IMAGE_TAG) {
            docker.push()
          }
        }
      }
    }
  }
}
