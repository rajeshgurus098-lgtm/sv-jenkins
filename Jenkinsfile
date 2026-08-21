pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-web-app"
        CONTAINER_NAME = "my-web-app-container"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Build Stage Completed'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Tests'

                sh '''
                if [ -f index.html ]; then
                    echo "index.html exists"
                else
                    echo "index.html missing"
                    exit 1
                fi
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker Image'

                sh '''
                docker build -t $IMAGE_NAME .
                docker images
                '''
            }
        }

        stage('Docker Run') {
            steps {
                echo 'Running Docker Container'

                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d --name $CONTAINER_NAME -p 8081:80 $IMAGE_NAME
                docker ps
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline Executed Successfully!'
        }

        failure {
            echo 'Pipeline Failed!'
        }
    }
}
