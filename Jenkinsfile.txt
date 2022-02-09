pipeline {
    agent any
    stages {
		stage('checkout') {
            steps {
				echo '###################### Checking out the Application ######################'
                sh 'mvn --version'
            }
        },
        stage('build') {
            steps {
				echo '###################### Building the Application ######################'
                sh 'mvn --version'
            }
        },
		stage('test') {
            steps {
				echo '###################### Testing the Application ######################'
                sh 'mvn --version'
            }
        },
		stage('deploy') {
            steps {
				echo '###################### Deploying the Application ######################'
                sh 'mvn --version'
            }
        },
		stage('cleanup') {
            steps {
				echo '###################### Cleaning up the Application ######################'
                sh 'mvn --version'
            }
        }
    }
}