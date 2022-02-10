
def logSeparator = "##########################################################################################"

def log(String message)
{
    echo "############################ [ ${message} ]  ############################"
}

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
				echo logSeparator
				log('Checking out the Application')
				echo 'JobName ${env.JOB_NAME} running ${env.BUILD_ID} on ${env.JENKINS_URL}'
				echo '${ANYPOINT_USER}'
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('compile') {
			steps {
				echo logSeparator
				echo '###################### Compile the Application ######################'
                sh 'mvn compile'
				echo logSeparator
            }
		}
        stage('build') {
            steps {
				echo logSeparator
				echo '###################### Building the Application ######################'
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('test') {
            steps {
				echo logSeparator
				echo '###################### Testing the Application ######################'
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('package') {
            steps {
				echo logSeparator
				echo '###################### Packaging the Application ######################'
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('deploy') {
            steps {
				echo logSeparator
				echo '###################### Deploying the Application ######################'
                sh 'mvn --version'
				 sh 'mvn deploy -DmuleDeploy -Danypoint.username=${ANYPOINT_USER} -Danypoint.password=${ANYPOINT_PASS}'
				 echo logSeparator
            }
        }
		stage('publish to artifact') {
            steps {
				echo logSeparator
				echo '###################### Publishing the Application to Artifact ######################'
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('cleanup') {
            steps {
				echo logSeparator
				echo '###################### Cleaning up the Application ######################'
                sh 'mvn --version'
				echo logSeparator
            }
        }
    }
}