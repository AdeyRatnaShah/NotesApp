pipeline {
    agent any

    environment {
        IMAGE_NAME = 'notes-app:latest'
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/AdeyRatnaShah/NotesApp'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerHubCreds') {
                        docker.image("${IMAGE_NAME}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }

    options {
        disableConcurrentBuilds()
    }
}
