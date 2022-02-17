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
    echo "############################ [ ${message} ]  ############################"
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

def validateTags(String TAG){
	echo "TAG: ${TAG}"
	
	if (!TAG?.trim()){
		
		echo "the string is null or empty."
		return '1.0.0'
	}else {
		echo "the string is NOT null or empty."
		lastTagNumber = (TAG.substring(TAG.lastIndexOf(".") + 1, TAG.length())).toInteger() +1
		firstTagNumber = TAG.substring(0, TAG.indexOf("."))
		middleTagNumber = TAG.substring(TAG.indexOf(".")+1, TAG.lastIndexOf("."))
		return firstTagNumber + '.' + middleTagNumber + '.' + lastTagNumber.toString()
	}
	

}

pipeline {
    agent any
	environment {
		ANYPOINT_CREDENTIALS = credentials('anypointplatform')
		ANYPOINT_USER = "${ANYPOINT_CREDENTIALS_USR}"
		ANYPOINT_PASS = "${ANYPOINT_CREDENTIALS_PSW}"
		MVN = "mvn"
        GIT = "git"
		MULE_SETTINGS = 'C:\\Users\\NelsonDias\\.m2\\settings.xml'
	}
    stages {
		stage ('initialize'){
			steps {
				echo logSeparator
				log('Launching Pipeline')
				launching(jobname, branch, jenkinsurl, buildid, buildnumber, buildurl, mavenhome, gitcommit, gitpreviouscommit)
				//print all variables
				//sh 'printenv'
			}
		}
		stage('checkout') {
            steps {
				script {
					echo logSeparator
					log('Checking out the Application')
					checkout([
						$class: 'GitSCM',
						branches: scm.branches,
						doGenerateSubmoduleConfigurations: scm.doGenerateSubmoduleConfigurations,
						extensions: scm.extensions,
						userRemoteConfigs: scm.userRemoteConfigs
					])
					sh "${GIT} status"
					echo "Multi-branch set to: ${branch}"
					echo logSeparator
				}
            }
        }
		stage('compile') {
			when {
				not{ 
					anyOf {
						environment ignoreCase: true, name: 'BRANCH_NAME', value: 'develop'
						environment ignoreCase: true, name: 'BRANCH_NAME', value: 'release'
						environment ignoreCase: true, name: 'BRANCH_NAME', value: 'master'
					}
				}
            }
			steps {
				echo logSeparator
				log('Compile the Application')
				
                sh "${MVN} clean compile"
				echo logSeparator
            }
		}
		stage('test') {
			when {
				not{ 
					anyOf {
						environment ignoreCase: true, name: 'BRANCH_NAME', value: 'develop'
						environment ignoreCase: true, name: 'BRANCH_NAME', value: 'release'
						environment ignoreCase: true, name: 'BRANCH_NAME', value: 'master'
					}
				}
            }
            steps {
				echo logSeparator
				log('Testing the Application')
                sh '${MVN} clean test -DanypointUsername=${ANYPOINT_USER} -DanypointPassword=${ANYPOINT_PASS} --settings ${MULE_SETTINGS}'
				echo logSeparator
            }
        }
		stage('build') {
			when {
				environment ignoreCase: false, name: 'BRANCH_NAME', value: 'develop'
            }
            steps {
				echo logSeparator
				log('Build the Application')
				sh '${MVN} clean install -DanypointUsername=${ANYPOINT_USER} -DanypointPassword=${ANYPOINT_PASS} --settings ${MULE_SETTINGS}'
				echo logSeparator
            }
        }
		stage('deploy') {
			when {
				environment ignoreCase: false, name: 'BRANCH_NAME', value: 'develop'
            }
            steps {
				echo logSeparator
				log('Deploying the Application')
				sh '''${MVN} clean deploy -DmuleDeploy -DbuildNumber=''' + buildnumber + ''' -DchangeList=-SNAPSHOT -Pcloudhub -Denv=dev -DskipTests -Dch.workers=1 -Dch.workerType=MICRO -DanypointUsername=${ANYPOINT_USER} -DanypointPassword=${ANYPOINT_PASS} --settings ${MULE_SETTINGS}'''
				 echo logSeparator
            }
        }
		stage('publish snapshot') {
			when {
				environment ignoreCase: false, name: 'BRANCH_NAME', value: 'develop'
            }
            steps {
				script {
					echo logSeparator
					log('Publishing the Artifactory Snapshot to Exchange')
					
					sh '''${MVN} clean install deploy -DbuildNumber=''' + buildnumber + ''' -DchangeList=-SNAPSHOT -Pexchange -DanypointUsername=${ANYPOINT_USER} -DanypointPassword=${ANYPOINT_PASS} --settings ${MULE_SETTINGS}'''
					echo logSeparator
				}
				
            }
        }
		stage('generate tag and publish release') {
			when {
				environment ignoreCase: false, name: 'BRANCH_NAME', value: 'release'
            }
            steps {
				script {
					// to use the readMavenPom function please install the plugin 'pipeline-utility-steps'
					//Then, Navigate to jenkins > Manage jenkins > In-process Script Approval: There is a pending command, which must be approved.
					pom = readMavenPom(file: 'pom.xml')
					projectVersion = pom.getVersion()
					projectArtifactId = pom.getArtifactId()
					echo logSeparator
					log('Publishing the Artifactory Release to Exchange')
					try {
						//You will need to change the credential confguration to fit your specific installation. Two examples are shown below.
						//'github' is the Jenkins Credential Id created for the Git repository
						//withCredentials([sshUserPrivateKey(credentialsId: 'github', keyFileVariable: 'GIT_KEY_FILE', passphraseVariable: 'GIT_PASSPHRASE', usernameVariable: 'GIT_USERNAME')]) {
						withCredentials([[$class: 'UsernamePasswordMultiBinding', 
							credentialsId: 'github', 
							usernameVariable: 'GIT_USERNAME', 
							passwordVariable: 'GIT_PASSWORD']]) {
							sh 'GIT_ASKPASS=true ${GIT} pull origin --tags'
							
							//sh "${GIT} tag ${projectVersion}"
							//sh "${GIT} tag 1.0.0"
							sh '${GIT} config credential.username ${GIT_USERNAME}' 
							sh "${GIT} config credential.helper '!echo password=\$GIT_PASSWORD; echo'"
							
							//tags = sh "GIT_ASKPASS=true ${GIT} tag  | grep -E '^[0-9]' | sort -V | tail -1"
							def tags = sh(returnStdout: true, script: "GIT_ASKPASS=true ${GIT} tag  | grep -E '^[0-9]' | sort -V | tail -1").trim()
							def tagVersion = validateTags(tags)
							log(tagVersion)
							sh "${GIT} tag ${tagVersion}"
							sh 'GIT_ASKPASS=true ${GIT} push origin --tags'
							def lastTagNumber = tagVersion.substring(tagVersion.lastIndexOf(".") + 1, tagVersion.length())
							sh '''${MVN} versions:set -DnewVersion=''' + tagVersion
							sh '''${MVN} clean install deploy -DbuildNumber=''' + lastTagNumber + ''' -DchangeList=''  -Pexchange -DanypointUsername=${ANYPOINT_USER} -DanypointPassword=${ANYPOINT_PASS} --settings ${MULE_SETTINGS}'''
							echo logSeparator
						}
                    } finally {
                        sh '${GIT} config --unset credential.username'
                        sh '${GIT} config --unset credential.helper'
                    }
					
					
				}
				
            }
        }
		/*stage('cleanup') {
            steps {
				script {
					echo logSeparator
					log('Cleaning up the Application')
                
					// clean up our workspace
					deleteDir()
					// clean up tmp directory
					dir("${workspace}@tmp") {
						deleteDir()
					}
					// clean up script directory
					dir("${workspace}@script") {
						deleteDir()
					}
					echo logSeparator
				}
            }
        }*/
    }
	/*post {
        // Clean after build
        always {
            echo logSeparator
					log('Cleaning up the Application')
                
					// clean up our workspace
					deleteDir()
					// clean up tmp directory
					dir("${workspace}@tmp") {
						deleteDir()
					}
					// clean up script directory
					dir("${workspace}@script") {
						deleteDir()
					}
					echo logSeparator
        }
    }*/
}