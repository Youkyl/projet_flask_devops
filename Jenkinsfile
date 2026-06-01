pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "projet-devops"
        DOCKER_TAG   = "${env.BUILD_NUMBER}"
    }
    stages {
        stage("Checkout") {
            steps {
                echo "Recuperation du code source..."
                checkout scm
            }
        }
        stage("Build Docker Image") {
            steps {
                echo "Construction de l image Docker..."
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }
        stage("Test") {
            steps {
                echo "Test de l application..."
                sh "docker run --rm ${DOCKER_IMAGE}:${DOCKER_TAG} python -c 'import flask; print(flask.__version__)'"
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploiement..."
                sh "docker compose down || true"
                sh "docker compose up -d --build"
            }
        }
    }
    post {
        success {
            echo "Pipeline termine avec succes !"
        }
        failure {
            echo "Echec du pipeline — verifiez les logs."
        }
    }
}