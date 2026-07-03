pipeline {
    agent any

    environment {
        IMAGE_NAME     = "hello-world-app"
        CONTAINER_NAME = "hello-world-container"
        DOCKER         = "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "\"%DOCKER%\" build -t %IMAGE_NAME% ."
            }
        }

        stage('Remove Old Container') {
            steps {
                sh "\"%DOCKER%\" rm -f %CONTAINER_NAME% || exit 0"
            }
        }

        stage('Run Container') {
            steps {
                sh "\"%DOCKER%\" run -d --name %CONTAINER_NAME% -p 80:80 %IMAGE_NAME%"
            }
        }

        stage('Verify') {
            steps {
                sh "\"%DOCKER%\" ps --filter name=%CONTAINER_NAME%"
            }
        }
    }

    post {
        success {
            echo "Deployed! Visit http://localhost"
        }
        failure {
            echo "Pipeline failed. Check logs above."
        }
    }
}
