pipeline {
	agent any
	tools {
		maven "MVN3.6"
		jdk "jdk8"
	}

	stages {
		stage('Fetch code'){
			steps {
			git branch: 'vp-rem', url:'https://github.com/devopshydclub/vprofile-repo.git'
			}
		}

		stage ('Build'){
			steps{
				sh 'mvn install -DskipTests'
			}
			post {
				success {
				   echo 'Now Archiving it....'
				   archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}

		stage ('Test') {
			steps{
				sh 'mvn test'
			}
		}

		stage('Checkstyle'){
			steps {
				sh 'mvn checkstyle:checkstyle'
			}
		}

		stage('Sonar Analysis'){
			environment {
			scannerHome = tool 'sonar4.7'
		  }
			steps{
				withSonarQubeEnv('sonar') {
				sh '''${scannerHome}/bin/sonar-scanner
				-Dsonar.projectKey=vprofile'''
				}
			}
		}
	}
}
