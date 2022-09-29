pipeline {
  agent any
  environment {
		registryCredential = 'dockerhub' 
	}
  stages {
    stage('Build') {
			steps {
				dir('microservices-class-prj'){
					sh 'docker compose build'
				}
			} 
		}
    stage('Test') {
      steps {
			dir('microservices-class-prj'){
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
						sh 'docker compose push mihmood/microservices-class-prj:latest'
					} 
				}
			} 
		}
	}
}
