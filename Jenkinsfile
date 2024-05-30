pipeline{
    agent any 
    tools{
        maven "Maven_3_5_2"
    }
    stages{
//         stage('CompileandRunSonarAnalysis') {
//             steps {	
// 		    withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
//     sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=tech365sec10 -Dsonar.organization=tech365sec10 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=$SONAR_TOKEN'
// }

		
// 			}
//         } 
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
                    [credentialsId:"dockerlogin", url: ""]
                )  {
                    script{
                    app = docker.build("asg")
                    }
                }
            }
        }

        stage('Push'){
            steps{
                script{
                    docker.withRegistry("https://211125486101.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:aws_credentials"){
                        app.push("latest")
                    }
                    
                    
                }
            }
        }
    }

    
}
