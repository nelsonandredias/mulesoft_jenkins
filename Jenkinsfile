pipeline {
    agent any
    stages {
		stage('checkout') {
			environment {
				ANYPOINT_CREDENTIALS = credentials('anypointplatform')
			}
            steps {
				echo '###################### Checking out the Application ######################'
				echo '${ANYPOINT_CREDENTIALS_USR}'
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