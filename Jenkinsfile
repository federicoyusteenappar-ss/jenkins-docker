def IMAGE_TAG = ""
def GIT_COMMIT = ""

pipeline {

    agent any
    
    parameters { 
        string(name: 'BRANCH_TAG', defaultValue: 'main', description: 'Branch or tag to build')
        string(name: 'IMAGE_NAME', defaultValue: 'flask-myapp', description: 'Docker image name') 
    }

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

                def variables = [
                    dockerfileDir: "./",
                    dockerfileName: "Dockerfile",
                    buildArgs: "",
                    image: "${IMAGE_NAME}:" + env.IMAGE_TAG
                ]

                def image = docker.build(variables.image, "${variables.buildArgs} ${variables.dockerfileDir} -f ${variables.dockerfileName}")

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