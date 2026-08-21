pipeline {
    agent any

    
    environment {
    IMAGE_NAME = "my-web-app"
    DOCKER = "/usr/bin/docker"
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
                $DOCKER build -t $IMAGE_NAME .
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
