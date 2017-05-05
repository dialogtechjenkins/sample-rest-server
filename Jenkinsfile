pipeline {

	agent any
    
    options {
    	buildDiscarder(logRotator(numToKeepStr:'10'))   // Keep the 10 most recent builds 
  	}

	stages {
	
		stage('Build') {
			steps {
				sh 'mvn clean package site'
				junit allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml'
			}
		}
		
		stage('Quality Analysis') {
			environment {
				SONAR = credentials('sonar')
			}
			when {
				branch 'master'
			}
			steps {
				parallel (
					"Integration Test" : {
						echo 'Run integration tests here...'
					},
					"Sonar Scan" : {
						sh "mvn sonar:sonar -Dsonar.host.url=http://sonar.beedemo.net:9000 -Dsonar.organization=$SONAR_USR -Dsonar.login=$SONAR_PSW"
					}, failFast: true
				)	
			}
		}
		
    }

}