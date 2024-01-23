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
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=alfonso84_testing_devsecops -Dsonar.organization=alfonso84 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=b91e53be856225ba01195f41378b2292b6e93a11'
			}
    }
   }

    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                }
                 
            }
        }
    }
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }    
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
                sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
         }
        }
      }

	// stage('Push') {
    //         steps {
    //             script{
    //                 docker.withRegistry('https://381050469176.dkr.ecr.eu-north-1.amazonaws.com', 'ecr:eu-north-1:aws-credentials') {
    //                 app.push("latest")
    //                 }
    //             }
    //         }
    // 	}
	   
	// stage('Kubernetes Deployment of ASG Bugg Web Application') {
	//    steps {
	//       withKubeConfig([credentialsId: 'kubelogin']) {
	// 	  sh('kubectl delete all --all -n devsecops')
	// 	  sh ('kubectl apply -f deployment.yaml --namespace=devsecops')
	// 	}
	//       }
   	// }
	   
	// stage ('wait_for_testing'){
	//    steps {
	// 	   sh 'pwd; sleep 180; echo "Application Has been deployed on K8S"'
	//    	}
	//    }
	   
	// stage('RunDASTUsingZAP') {
    //       steps {
	// 	    withKubeConfig([credentialsId: 'kubelogin']) {
	// 			sh('zap.sh -cmd -quickurl http://$(kubectl get services/asgbuggy --namespace=devsecops -o json| jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
	// 			archiveArtifacts artifacts: 'zap_report.html'
	// 	    }
	//      }
    //    } 
  }
