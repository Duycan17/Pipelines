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
                    dir('client') {
                        sh 'docker build -t react-app .'
                    }
                }
            }
        }

        stage('Build Spring Boot App') {
            steps {
                script {
                    dir('server') {
                        sh './gradlew bootJar'
                        sh 'docker build -t spring-app .'
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
