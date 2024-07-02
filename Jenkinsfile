pipeline {
    agent any

    environment {
        DOCKER_BUILDKIT = 1
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build React App') {
            steps {
                script {
                    dir('vite-project') {
                        sh 'docker build -t react-app -f Dockerfile .'
                    }
                }
            }
        }

        stage('Build Spring Boot App') {
            steps {
                script {
                    dir('demo1') {
                        sh './gradlew bootJar'
                        sh 'docker build -t spring-app -f Dockerfile .'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker system prune -f'
        }
    }
}
