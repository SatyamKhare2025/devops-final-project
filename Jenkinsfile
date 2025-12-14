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
                  docker build -t devops-flask:ci .
                '''
            }
        }

        stage('Load Image into Minikube') {
            steps {
                sh '''
                  minikube image load devops-flask:ci
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
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                  kubectl apply -f k8s/deployment.yaml
                  kubectl rollout status deployment/devops-flask
                 '''
           }
       }

    }
}

