pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('c997079f-276d-4b33-bcd9-f0e9d78ed439')
    }

    triggers {
        pollSCM('*/5 * * * *') 
    }

    stages {
        stage('Checkout') {
            agent any
            steps {
                checkout scm
            }
        }

        stage('Init') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Build') {
         steps {
         sh "docker build --build-arg asmaarrak/books -t ${imageName} ."         }
}


        stage('Deliver') {
            steps {
                sh 'docker push asmaarrak/books:latest'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker rmi asmaarrak/books:$BUILD_ID'
                sh 'docker logout'
            }
        }
    }
}
