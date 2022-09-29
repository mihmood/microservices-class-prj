pipeline {
  agent any
  environment {
		registryCredential = 'dockerhub' 
	}
  stages {
    stage('Build') {
			steps {
				dir('backend-dice-class-prj'){
					sh 'docker compose down'
					sh 'sleep 5'
					sh 'docker compose build'
				}


				dir('frontend-dice-class-prj'){
                                        sh 'docker compose down'
                                        sh 'sleep 5'
                                        sh 'docker compose build'
                                }
			} 
		}
    stage('Test') {
      			steps {
				dir(''){
					sh 'docker compose up -d' 
					sh 'sleep 5'
					sh 'curl -I http://ec2-18-206-174-55.compute-1.amazonaws.com:9091/student/3'
			}
		} 
	}
    stage('Publish') {
			steps{
				script {
					docker.withRegistry( '', registryCredential ) {
						sh 'docker compose push mihmood/frontend-python:latest'
					} 
				}
			} 
		}
	}
    post {
                        success {
                                slackSend color: "good", message: "Frontend Pipeline passed ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
                        }
                        failure {
                                slackSend failOnError: true, message: "Frontend Build failed ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
                        }
                }
}
