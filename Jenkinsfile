pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'projet-devops'
        DOCKER_TAG   = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Récupération du code source...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Construction de l image Docker...'
                bat "docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% ."
            }
        }

        stage('Run Container') {
            steps {
                echo 'Lancement du conteneur...'
                bat "docker run -d -p 5000:5000 --name %DOCKER_IMAGE%-%DOCKER_TAG% %DOCKER_IMAGE%:%DOCKER_TAG%"
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'Échec du pipeline — vérifiez les logs.'
        }
    }
}