pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        APP_PORT = '8090' // le port de l'app Spring Boot
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                // Build du projet Spring Boot
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Run App') {
                    steps {
                        script {
                            // Stop previous instance
                            sh "pkill -f 'java.*demo-0.0.1-SNAPSHOT.jar' || true"
                            // Run the Spring Boot app
                            sh "nohup java -jar target/demo-0.0.1-SNAPSHOT.jar --server.port=${APP_PORT} > app.log 2>&1 &"
                        }
                    }
                }

        stage('Expose with Ngrok') {
            steps {
                script {
                    // Stopper l’ancien ngrok
                    sh 'pkill ngrok || true'
                    // Lancer ngrok
                    sh "nohup ngrok http ${APP_PORT} > ngrok.log 2>&1 & sleep 5"
                    // Récupérer l’URL publique ngrok et l’afficher dans les logs
                    sh '''
                        curl http://127.0.0.1:4040/api/tunnels > ngrok_tunnels.json
                        echo "=== URL publique Ngrok ==="
                        cat ngrok_tunnels.json | grep -o '"public_url":"[^"]*' | cut -d '"' -f4
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
