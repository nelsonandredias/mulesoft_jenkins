pipeline {
    agent any
    stages {
		stage('checkout') {
			environment {
				ANYPOINT_CREDENTIALS = credentials('anypointplatform')
			}
            steps {
				echo '###################### Checking out the Application ######################'
				sh  'echo ${ANYPOINT_CREDENTIALS_USR}'
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
				 sh 'mvn deploy -DmuleDeploy -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW}'
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