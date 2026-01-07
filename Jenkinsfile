pipeline {
    agent any

    environment {
        IMAGE_NAME = "sumitdorugade/cicd-app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Sumit19D/Docker-Zero-to-Hero.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
                
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: '2eba4b4b-d200-44ab-9ce6-f622aa7f10c2',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml --validate=false
                kubectl apply -f service.yaml --validate=false
                '''
            }
        }
    }
}

