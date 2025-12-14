pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                  cd app
                  eval $(minikube docker-env)
                  docker build -t devops-flask:ci .
                '''
            }
        }

        stage('Test Container') {
            steps {
                sh '''
                  docker run -d -p 5001:5000 --name flask-ci-test devops-flask:ci
                  sleep 5
                  curl http://localhost:5001/health
                  docker rm -f flask-ci-test
                '''
            }
        }
    }
}
