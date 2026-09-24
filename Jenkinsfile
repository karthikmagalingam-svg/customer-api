
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Java application...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t customer-api:latest .'
            }
        }

        stage('Docker Run') {
            steps {
                echo 'Starting Docker container...'

                sh '''
                    docker stop customer-api || true
                    docker rm customer-api || true

                    docker run -d \
                      --name customer-api \
                      -p 8080:8080 \
                      customer-api:latest
                '''
            }
        }
    }

    post {
        success {
            echo '================================='
            echo 'CI/CD PIPELINE SUCCESSFUL'
            echo '================================='
        }

        failure {
            echo '================================='
            echo 'CI/CD PIPELINE FAILED'
            echo '================================='
        }
    }
}
