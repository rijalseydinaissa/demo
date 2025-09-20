pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'Java17'
    }

    environment {
        DOCKER_IMAGE = "demo-springboot"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/TON_USER/TON_REPO.git'
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

        stage('Run with Docker') {
            steps {
                sh 'docker rm -f demo-app || true'
                sh 'docker run -d --name demo-app -p 8080:8080 $DOCKER_IMAGE'
            }
        }
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
