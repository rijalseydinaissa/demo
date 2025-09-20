pipeline {
    agent any

    tools {
        maven 'Maven-3.9.0'
        jdk 'JDK-17'
    }

    environment {
        DOCKER_IMAGE = "demo-springboot"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        /*stage('Run with Docker') {
            steps {
                sh 'docker rm -f demo-app || true'
                sh 'docker run -d --name demo-app -p 8080:8080 $DOCKER_IMAGE'
            }
        }*/
    }

    post {
        success {
            echo 'Déploiement réussi 🎉'
        }
        failure {
            echo 'Échec du pipeline ❌'
        }
    }
}
