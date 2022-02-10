def branch = env.BRANCH_NAME
def jobname = env.JOB_NAME
def jenkinsurl = env.JENKINS_URL
def buildid = env.BUILD_ID
def buildnumber = env.BUILD_NUMBER
def buildurl = env.BUILD_URL
def mavenhome = env.MAVEN_HOME
def gitcommit = env.GIT_COMMIT
def gitpreviouscommit = env.GIT_PREVIOUS_SUCCESSFUL_COMMIT

def logSeparator = '###########################################################################################'

def log(String message)
{
    echo '############################ [ ${message} ]  ############################'
}


def launching(String JOB_NAME, String BRANCH, String JENKINS_URL, String BUILD_ID, String BUILD_NUMBER, String BUILD_URL, String MAVEN_HOME, String GIT_COMMIT, String GIT_PREVIOUS_COMMIT)
{
	echo "JOB_NAME: ${JOB_NAME}"
	echo "BRANCH: ${BRANCH}"
	echo "JENKINS_URL: ${JENKINS_URL}"
	echo "BUILD_ID: ${BUILD_ID}"
	echo "BUILD_NUMBER: ${BUILD_NUMBER}"
	echo "BUILD_URL: ${BUILD_URL}"
	echo "MAVEN_HOME: ${MAVEN_HOME}"
	echo "GIT_COMMIT: ${GIT_COMMIT}"
	echo "GIT_PREVIOUS_COMMIT: ${GIT_PREVIOUS_COMMIT}"
}

pipeline {
    agent any
	environment {
		ANYPOINT_CREDENTIALS = credentials('anypointplatform')
		ANYPOINT_USER = "${ANYPOINT_CREDENTIALS_USR}"
		ANYPOINT_PASS = "${ANYPOINT_CREDENTIALS_PSW}"
	}
    stages {
		stage ('launching'){
			steps {
				echo logSeparator
				log('Launching Pipeline')
				launching(jobname, branch, jenkinsurl, buildid, buildnumber, buildurl, mavenhome, gitcommit, gitpreviouscommit)
			}
		}
		stage('checkout') {
            steps {
				echo logSeparator
				log('Checking out the Application')
				echo '${ANYPOINT_USER}'
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('compile') {
			steps {
				echo logSeparator
				log('Compile the Application')
				sh 'printenv'
                sh 'mvn compile'
				echo logSeparator
            }
		}
        stage('build') {
            steps {
				echo logSeparator
				log('Building the Application')
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('test') {
            steps {
				echo logSeparator
				log('Testing the Application')
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('package') {
            steps {
				echo logSeparator
				log('Packaging the Application')
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('deploy') {
            steps {
				echo logSeparator
				log('Deploying the Application')
                sh 'mvn --version'
				// sh 'mvn deploy -DmuleDeploy -Danypoint.username=${ANYPOINT_USER} -Danypoint.password=${ANYPOINT_PASS}'
				 echo logSeparator
            }
        }
		stage('publish to artifact') {
            steps {
				echo logSeparator
				log('Publishing the Application to Artifact')
                sh 'mvn --version'
				echo logSeparator
            }
        }
		stage('cleanup') {
            steps {
				echo logSeparator
				log('Cleaning up the Application')
                sh 'mvn --version'
				echo logSeparator
            }
        }
    }
}