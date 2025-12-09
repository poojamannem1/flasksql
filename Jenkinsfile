pipeline {
    agent any

    stages {
        stage('clone repo') {
            steps {
                git branch: 'main', url: 'https://github.com/poojamannem1/flasksql.git'
            }
        }
        stage('Build image') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }
        stage('deploy with docker compose') {
            steps {
                sh 'docker-compose down || true '
                
                sh 'docker-compose up -d --build'
            }
        }
       
    }
}
