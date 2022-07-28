// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Test') {
// 		echo "Test"
// 	}
// }

pipeline {
	agent any
	environment {
		dockerHome = tool 'infi-docker'
		mavenHome = tool 'infi-maven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		stage ('Build') {
			steps {
				sh 'mvn --version'
				sh 'docker version'				
				echo "Build"
			}
		}
		stage ('Complie') {
			steps {
				sh "mvn clean compile"
			}
		}
		stage ('Test') {
			steps {
				sh "mvn test"
			}
		}
		stage ('Package') {
			steps {
				sh "mvn package -DskipTests"
			}
		}
		stage ('Build Docker Image') {
			steps {
				script {
					dockerImage = docker.build("sarvmr/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}
		stage ('Push Docker Image') {
			steps {
				script {
					docker.withRegistry('' , 'infi-dockerhub'){
						dockerImage.push();
						dockerImage.push('latest');
					}
				}				
			}
		}
	}
}
