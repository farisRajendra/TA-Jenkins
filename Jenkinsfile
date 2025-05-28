pipeline {
    agent any

    environment {
        COMPOSE_CMD = 'docker-compose'
    }

    stages {
        stage('Checkout Repo') {
            steps {
                git branch: 'master', url: 'https://github.com/farisRajendra/TA-Jenkins.git'
            }
        }

        stage('Build and Run Containers') {
            steps {
                // Hentikan dan hapus container jika ada
                sh "${COMPOSE_CMD} down -v || true"
                // Bangun dan jalankan container
                sh "${COMPOSE_CMD} up -d --build"
                // Tunggu beberapa detik untuk memastikan database siap
                sleep(time: 15, unit: 'SECONDS')
            }
        }

        stage('Laravel Setup') {
            steps {
                // Install dependencies
                sh "${COMPOSE_CMD} exec app composer install"
                // Generate key
                sh "${COMPOSE_CMD} exec app php artisan key:generate"
                // Jalankan migrasi
                sh "${COMPOSE_CMD} exec app php artisan migrate --force"
            }
        }
    }
}