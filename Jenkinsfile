pipeline {
    agent any
	environment {
		ANYPOINT_CREDENTIALS = credentials('anypointplatform')
		ANYPOINT_USER = "${ANYPOINT_CREDENTIALS_USR}"
		ANYPOINT_PASS = "${ANYPOINT_CREDENTIALS_PSW}"
	}
    stages {
		stage('checkout') {
            steps {
				echo '###################### Checking out the Application ######################'
				echo 'JobName ${env.JOB_NAME} running ${env.BUILD_ID} on ${env.JENKINS_URL}'
				echo '${ANYPOINT_USER}'
                sh 'mvn --version'
            }
        }
		stage('compile') {
			steps {
				echo '###################### Compile the Application ######################'
                sh 'mvn compile'
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
		stage('package') {
            steps {
				echo '###################### Packaging the Application ######################'
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
		stage('publish to artifact') {
            steps {
				echo '###################### Publishing the Application to Artifact ######################'
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