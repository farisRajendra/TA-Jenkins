pipeline {
    agent any

    environment {
        LARAVEL_APP_NAME = "laravel-app"
        DB_CONTAINER_NAME = "laravel-mysql"
        DB_NAME = "laravel"
        DB_USER = "root"
        DB_PASSWORD = "password"
        DB_ROOT_PASSWORD = "password"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/farisRajendra/TA-Jenkins.git'
            }
        }

        stage('Build Laravel Docker Image') {
            steps {
                sh "docker build -t ${LARAVEL_APP_NAME} ."
            }
        }

        stage('Run MySQL Container') {
            steps {
                sh """
                docker run -d \
                  --name ${DB_CONTAINER_NAME} \
                  -e MYSQL_DATABASE=${DB_NAME} \
                  -e MYSQL_USER=${DB_USER} \
                  -e MYSQL_PASSWORD=${DB_PASSWORD} \
                  -e MYSQL_ROOT_PASSWORD=${DB_ROOT_PASSWORD} \
                  -p 3306:3306 \
                  mysql:8.0
                """
                sleep(time: 20, unit: 'SECONDS')  // Tunggu MySQL siap
            }
        }

        stage('Run Laravel Container') {
            steps {
                sh """
                docker run -d \
                  --name ${LARAVEL_APP_NAME} \
                  --link ${DB_CONTAINER_NAME}:mysql \
                  -e DB_HOST=mysql \
                  -e DB_DATABASE=${DB_NAME} \
                  -e DB_USERNAME=${DB_USER} \
                  -e DB_PASSWORD=${DB_PASSWORD} \
                  -p 8000:8000 \
                  ${LARAVEL_APP_NAME}
                """
            }
        }

        stage('Setup Laravel') {
            steps {
                sh """
                docker exec ${LARAVEL_APP_NAME} composer install
                docker exec ${LARAVEL_APP_NAME} php artisan key:generate
                docker exec ${LARAVEL_APP_NAME} php artisan migrate --force
                """
            }
        }
    }
}