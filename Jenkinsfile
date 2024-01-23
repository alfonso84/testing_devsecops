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
               }
            }
    }

  }
}