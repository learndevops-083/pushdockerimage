pipeline {
    agent any
    environment{
        DOCKERHUB_CREDS = credentials('dockerhub')
    }
    stages {
        stage('Clone Repo') {
            steps {
                checkout scm
                sh 'ls *'
            }
        }
        stage('Build Image') {
            steps {
                //sh 'docker build -t joemile/jenkinsdocker ./pushdockerimage/' (this will use the tag latest)
		sh 'docker build -t joemile/jenkinsdocker:$BUILD_NUMBER ./pushdockerimage/'
            }
        }
        stage('Docker Login') {
            steps {
                //sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW' (this will leave the password visible)
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'                
                }
            }
        stage('Docker Push') {
            steps {
		//sh 'docker push joemile/jenkinsdocker' (this will use the tag latest)    
                sh 'docker push joemile/jenkinsdocker:$BUILD_NUMBER'
                }
            }
        }
    post {
		always {
			sh 'docker logout'
		}
	 }
    }

