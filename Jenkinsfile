pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        APP_NAME = "myapp"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/jayantdanech/project_devops_v1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $REGISTRY/$APP_NAME:latest ./app'
            }
        }

        stage('Push to Local Registry') {
            steps {
                sh 'docker push $REGISTRY/$APP_NAME:latest'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'docker tag $REGISTRY/$APP_NAME:latest $REGISTRY/$APP_NAME:staging'
                sh 'docker push $REGISTRY/$APP_NAME:staging'
                sh 'docker compose -f ../app/docker-compose.yml up -d staging'
            }
        }

        stage('Deploy to UAT') {
            steps {
                input message: "Deploy to UAT?"
                sh 'docker tag $REGISTRY/$APP_NAME:latest $REGISTRY/$APP_NAME:uat'
                sh 'docker push $REGISTRY/$APP_NAME:uat'
                sh 'docker compose -f ../app/docker-compose.yml up -d uat'
            }
        }

        stage('Deploy to Production') {
            steps {
                input message: "Deploy to Production?"
                sh 'docker tag $REGISTRY/$APP_NAME:latest $REGISTRY/$APP_NAME:prod'
                sh 'docker push $REGISTRY/$APP_NAME:prod'
                sh 'docker compose -f ../app/docker-compose.yml up -d prod'
            }
        }
    }
}

