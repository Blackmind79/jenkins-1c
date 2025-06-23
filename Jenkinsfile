pipeline {
    // agent any
    agent 1c

    environment {
		// GLOBAL_VAR = "Test variable"
    }	
    stages {
        
		stage('Vanessa tests') {
			environment {
                // Create credentials in Jenkins Global credentials store
                REMOTE_INFOBASE_LOGIN = credentials('REMOTE_INFOBASE_LOGIN')
                // Uncomment and create creds if it is not empty password
                // REMOTE_INFOBASE_PASSWORD = credentials('REMOTE_INFOBASE_PASSWORD')
                REMOTE_INFOBASE_PASSWORD = ""
                REMOTE_INFOBASE_SRVR = credentials('REMOTE_INFOBASE_SRVR')
                REMOTE_INFOBASE_REF = credentials('REMOTE_INFOBASE_REF')
                // Combined envirmoents variables
                REMOTE_INFOBASE_PATH="Srvr=\"${REMOTE_INFOBASE_SRVR}\";Ref=\"${REMOTE_INFOBASE_REF}\";"
			}
            steps {
                echo 'Vanessa Testing ...'
                echo 'Prepare parans from template'
                sh '''
                    envsubst < ./vanessa/VAParams.json.template > ./vanessa/VAParams.json
                '''
                echo 'Run vanessa-automation tests'
                sh '''
                    "/opt/1cv8/x86_64/8.3.26.1498/1cv8c"\
                      /N"${REMOTE_INFOBASE_LOGIN}" \
                      /P"${REMOTE_INFOBASE_PASSWORD}" \
                      /TestManager \
                      /Execute "/usr/share/oscript/lib/vanessa-automation/vanessa-automation.epf" \
                      /IBConnectionString "${REMOTE_INFOBASE_PATH}" \
                      /C"StartFeaturePlayer;QuietInstallVanessaExt;VAParams=./vanessa/VAParams.json"
                '''
                echo '...done'
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
                echo '...done'
            }
        }
		
        stage('Generate reports in Allure') {
            steps {
                echo 'Generate reports in Allure ...'
                echo '...done'
            }
        }
    }
}