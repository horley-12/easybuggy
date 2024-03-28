pipeline{
    agent any 
    tools{
        maven "Maven_3_5_2"
    }
    stages{
        stage('CompileandRunSonarAnalysis') {
            steps {	
		    withCredentials([string(credentialsId: 'HORLEY_TOKEN', variable: 'HORLEY_TOKEN')]) {
    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=horleysec1 -Dsonar.organization=horleysec1 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=$HORLEY_TOKEN'
}

		
			}
        } 
	     stage('RunSCAAnalysisUsingSnyk') {
             steps {		
			 	withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
			 		sh 'mvn snyk:test -fn'
			 	}
			 }
     }	
        stage('Build'){
            steps{
                withDockerRegistry(
                    [credentialsId:"horleydockerlogin", url: ""]
                )  {
                    script{
                    app = docker.build("horleyasg")
                    }
                }
            }
        }

        stage('Push'){
            steps{
                script{
                    docker.withRegistry("https://992382399201.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:horley-aws-credentials"){
                        app.push("latest")
                    }
                    
                    
                }
            }
        }
    }

    
}
