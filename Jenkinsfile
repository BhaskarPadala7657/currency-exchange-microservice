pipeline {
	agent any
	//agent { docker {image 'maven 3.6.3'}}
	environment{
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage('Checkout') {
			steps{
			sh 'mvn --version'
			sh 'docker version'
			echo "Build"
			echo "PATH - $PATH"
			echo "BUILD NUMBER - $env.BUILD_NUMBER"
			echo "BUILD ID - $env.BUILD_ID"
			echo "BUILD JOB - $env.BUILD_JOB"
			echo "BUILD TAG - $env.BUILD_TAG"
			echo "BUILD URL - $env.BUILD_URL"
			}
		}
		stage("Compile"){
			steps {
				sh "mvn clean compile"
			}
		}
		stage('Test') {
			steps{
				sh "mvn test"
			}
		}
		stage('Integration Test') {
			steps {
				echo "Integration Test"
			//echo "mvn failsafe:integration-test failsafe:verify"
			}
		}
		stage('Package') {
			steps {
			 sh "mvn package -DskipTests"
			}
		}
		stage('Build Docker Image') {
			steps {
				// "docker Build -t bapdala/currency-exchange-microservice:$env.BUILD_TAG"
				script{
					dockerImage = docker.Build("bpadala/currency-exchange-microservice:${env.BUILD_TAG}")
				}
			}
		}
		stage('Docker Image push') {
			steps{
				script{
					docker.withRegistry('',dockerhub){
						dockerImage.push();
						dockerImage.push('latest');
				}
			}
		}
			
		}


	} 
	
	post {
		always {
			echo 'I m awesome I run always'
		}
		success {
			echo 'I  run when successful'
		}
		failure {
			echo 'I  run when I fail'
		}
		
	}

}


