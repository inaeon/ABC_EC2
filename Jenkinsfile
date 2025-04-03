
pipeline {
	agent any 
	environment {
		IMAGE_NAME = "avdeshsainger/abctechnologies"
	}
	
	
	stages {
		stage('code checkout') {
			steps {
				git branch: 'main', url: 'https://github.com/inaeon/ABC.git'
			}
		}
		stage('code compile') {
			steps {
				sh 'mvn clean compile'
			}
		}
		stage('code test') {
			steps {
				sh 'mvn test'
			}
		}
		stage('code package'){
			steps {
				sh 'mvn package'
			}
		}
		stage('build docker image') {
			steps {
				sh 'cp target/ABCtechnologies-1.0.war .'
				sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
			}
		}
		stage('push docker image') {
			steps {
				withDockerRegistry([credentialsId: "hub.docker.com", url: ""]) {
					sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
				}
			}
		}
		stage('deployment') {
			steps {
				script {
					// Checking the previous container functionality
					// if it is running, stop and delete the old container
					// else if it is stopped, just delete it
					// else at last create a new container on the same port
					
					
					sh """
					if [ \$(docker ps -q -f name=abc_project) ]; then
						docker stop abc_project
						docker rm abc_project
						
					elif [ \$(docker ps -aq -f name=abc_project) ]; then
						docker rm abc_project
						
					fi
					docker run -itd -p 8081:8080 --name abc_project ${IMAGE_NAME}:${BUILD_NUMBER}
					"""			
				}
			}
		}
	
	
	
	}
	

}