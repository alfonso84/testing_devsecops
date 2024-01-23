pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=alfonso84_testing_devsecops -Dsonar.organization=asgbuggyalfonso84webapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=b91e53be856225ba01195f41378b2292b6e93a11'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://145988340565.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	   
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
}