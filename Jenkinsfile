pipeline {
    agent any

    environment {
        DOCKER_BUILDKIT = 0
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()  // prevents old build issues
            }
        }

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/poojamannem1/flasksql.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                        echo "🔨 Building Docker Image..."
                        docker build --no-cache -t flask-app .
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                  sh '''
                    set -e  # Stop on error
                    echo "📌 Stopping old containers..."
                    docker compose down || true

                    echo "🚀 Starting new containers..."
                    docker compose up -d --build

                    echo "📜 Showing running containers..."
                    docker ps
                  '''
                }
            }
        }
    }

    post {
        always {
            echo "🧹 Running cleanup..."

            // Removes unused containers, images, cache
            sh 'docker system prune -f || true'
        }
        success {
            echo "✅ Deployment Completed Successfully!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
