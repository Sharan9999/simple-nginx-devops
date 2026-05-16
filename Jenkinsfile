pipeline {
    agent any

    environment {
        IMAGE_NAME = "sharan9999/nginx-project:v1"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/sharan9999/simple-nginx-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Image') {
            steps {

                withCredentials([usernamePassword(
                credentialsId: 'dockerhub',
                usernameVariable: 'USER',
                passwordVariable: 'PASS')]) {

                sh '''
                docker login -u $USER -p $PASS
                docker push $IMAGE_NAME
                '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
