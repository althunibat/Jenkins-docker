pipeline {
    agent any
    stages {
         stage('pull'){
            steps{
                sh 'echo "checkout repository"'
                checkout scm   
            }
        }
         stage('Building Image') {
            steps {
                sh 'make build'
            }
        }
        stage('Tag and publish release image') {
            steps {
                sh 'make tag latest \$(git rev-parse --short HEAD) \$(git tag --points-at HEAD)'
                sh 'make buildtag master \$(git tag --points-at HEAD)'
                withEnv(["DOCKER_USER=${DOCKER_USER}",
                          "DOCKER_PASSWORD=${DOCKER_PASSWORD}"]){
                              sh 'make login'
                          }
                sh 'make publish'
            }
        }
    }
    post {
        always {
            sh 'echo "cleaning up"'
            sh 'make clean'
            sh 'make logout'
        }
    }
}