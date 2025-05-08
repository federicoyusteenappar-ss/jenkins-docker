def IMAGE_TAG = ""
def GIT_COMMIT = ""

pipeline {

    agent any

    environment {

        GIT_URL = 'https://github.com/federicoyusteenappar-ss/jenkins-docker'
    }

    stages {

        stage('Checkout SCM') {
            steps {
                script {

                    def scmVar = checkout scm: [$class: 'GitSCM', 
                    userRemoteConfigs: [[url: env.GIT_URL]], 
                    branches: [[name: '$BRANCH_TAG']]], poll: false
                    env.GIT_COMMIT = scmVar.GIT_COMMIT

                }
            }
        }

        stage('Build Docker image') {
            steps {
                script {

                 if("$BRANCH_TAG" == 'main'){
                    env.IMAGE_TAG = "latest"
                 }
                 else if("$BRANCH_TAG" == 'develop'){
                    env.IMAGE_TAG = "develop-" + env.GIT_COMMIT
                 } 
                 else {
                     env.IMAGE_TAG = "${BRANCH_TAG}"
                 }
                 
                 sh 'docker build -t flask-myapp:${IMAGE_TAG} .'

               }
            }
        }

        stage('Push docker image') {
            steps {
                echo 'Pushing docker image...'
            }
        }
    }
}