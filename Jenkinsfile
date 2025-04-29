pipeline {
    agent any
    
    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AnkitPrakash12C/docker_project.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("notes-app:latest")
                }
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerHubCreds') {
                        docker.image("notes-app:latest").push()
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'docker-compose down'
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
}
