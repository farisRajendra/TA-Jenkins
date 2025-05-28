pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = 'laravelapp'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'master', url: 'https://github.com/farisRajendra/TA-Jenkins.git'
            }
        }

        stage('Build and Run Docker Compose') {
            steps {
                sh 'docker-compose down -v || true'
                sh 'docker-compose up -d --build'
            }
        }

        stage('Setup Laravel') {
            steps {
                sh 'docker exec laravel-app php artisan key:generate'
                sh 'docker exec laravel-app php artisan migrate'
            }
        }
    }
}
