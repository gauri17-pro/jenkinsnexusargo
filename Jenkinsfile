pipeline {
    agent any
    
    stages {
         stage('Building') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Sonarqube Analysis') {
              steps {
                  //  withSonarQubeEnv(credentialsId: 'sonar-test', installationName: 'http://172.26.21.117:9000') {
                      withSonarQubeEnv('SonarQube') {
               //     sh 'mvn clean install sonar:sonar -Dsonar.projectKey=com.tom:sonarqube-jacoco-code-coverage -Dsonar.host.url=http://172.26.21.117:9000/ -Dsonar.login=loginHASH -Dsonar.sources=src/main/java/ -Dsonar.java.binaries=target/classes'
               //      sh 'mvn clean sonar:sonar -Dsonar.java.binaries=target/classes'
                       sh 'mvn clean package sonar:sonar -e -Dsonar.projectKey=javapetclinicpipeline -Dsonar.host.url=http://172.26.21.117:9000 -Dsonar.login=289d65cc17aa4a6ba4ea0be58f58674f9090a09b'
                    }
                    timeout(time: 30, unit: 'MINUTES') { 
                    waitForQualityGate abortPipeline: true 
                    }
               }
        }
		
		stage('Dockerization'){
				steps{
					script{
						
								docker.withRegistry('http://172.26.21.156:8085/v2', 'nexus-local'){
								sh 'docker build -t petclinic:latest .'
								sh 'docker push http://172.26.21.156:8085/v2/petclinic:latest'
							}
						
							
					}
				}
		}
        
    }   
}


