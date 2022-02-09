pipeline {
    agent any
	environment {
		ANYPOINT_CREDENTIALS = credentials('anypointplatform')
		ANYPOINT_USER = ${ANYPOINT_CREDENTIALS_USR}
		ANYPOINT_PASS = ${ANYPOINT_CREDENTIALS_PSW}
	}
    stages {
		stage('checkout') {
			environment {
				ANYPOINT_CREDENTIALS = credentials('anypointplatform')
			}
            steps {
				echo '###################### Checking out the Application ######################'
				sh  'echo ${ANYPOINT_USER}'
                sh 'mvn --version'
            }
        }
        stage('build') {
            steps {
				echo '###################### Building the Application ######################'
                sh 'mvn --version'
            }
        }
		stage('test') {
            steps {
				echo '###################### Testing the Application ######################'
                sh 'mvn --version'
            }
        }
		stage('deploy') {
            steps {
				echo '###################### Deploying the Application ######################'
                sh 'mvn --version'
				 sh 'mvn deploy -DmuleDeploy -Danypoint.username=${ANYPOINT_USER} -Danypoint.password=${ANYPOINT_PASS}'
            }
        }
		stage('cleanup') {
            steps {
				echo '###################### Cleaning up the Application ######################'
                sh 'mvn --version'
            }
        }
    }
}