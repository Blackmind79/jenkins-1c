pipeline {
    agent any

    environment {
		GLOBAL_VAR = "Test variable"
    }	
    stages {
        
		stage('Vanessa tests') {
            steps {
                echo 'Vanessa Testing ...'
            }
        }

		stage('SonarQube check') {
			environment {
				SONAR_TOKEN = credentials('SONAR_JENKINS_1C_TOKEN')
				PATH = "${SONAR_SCANNER_HOME}/bin:${PATH}"
			}
            steps {
                echo 'SonarQube Testing ...'
				sh 'printenv PATH'
                sh '''
					sonar-scanner -Dsonar.projectKey=jenkins-1c -Dsonar.sources=. -Dsonar.host.url=http://host.docker.internal:9000
                '''				
            }
        }
		
        stage('Export to Allure') {
            steps {
                echo 'Deploying to Allure ...'
            }
        }
    }
}