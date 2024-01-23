pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
        snyk 'snyk'
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=alfonso84_testing_devsecops -Dsonar.organization=alfonso84 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=b91e53be856225ba01195f41378b2292b6e93a11'
			}
    }
	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 sh 'docker build -t innoqadevsecops .'
                 sh 'docker tag innoqadevsecops:latest 381050469176.dkr.ecr.eu-north-1.amazonaws.com/innoqadevsecops:latest'
                 sh 'docker push 381050469176.dkr.ecr.eu-north-1.amazonaws.com/innoqadevsecops:latest'
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