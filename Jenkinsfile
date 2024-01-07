pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('c997079f-276d-4b33-bcd9-f0e9d78ed439')
    }

    triggers {
        pollSCM('*/5 * * * *') // Vérifier toutes les 5 minutes
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
                // Permet l'authentification
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Build') {
            steps {
                // Assuming Dockerfile is in the 'routes' folder
                sh 'docker build -t asmaarrak/books:latest -f routes/Dockerfile .'
            }
        }

        stage('Deliver') {
            steps {
                sh 'docker push asmaarrak/books:latest'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker rmi $DOCKERHUB_CREDENTIALS_USR/books:$BUILD_ID'
                sh 'docker logout'
            }
        }
    }
}
