node {
    checkout scm
    def customImage = docker.build("flask-myapp:${env.BUILD_TAG}")

}