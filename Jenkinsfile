pipeline {
    agent any

    environment {
        MAVEN_OPTS = '-Dmaven.repo.local=.m2/repository'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Compile & Package Microservices') {
            agent {
                image 'maven:3.9.6-eclipse-temurin-17'
            }
            steps {
                // Build each service in its specific architectural order
                dir('config-server') { sh 'mvn clean package -DskipTests' }
                dir('service-registry') { sh 'mvn clean package -DskipTests' }
                dir('zipkin-server') { sh 'mvn clean package -DskipTests' }
                dir('api-gateway') { sh 'mvn clean package -DskipTests' }
                dir('question-service') { sh 'mvn clean package -DskipTests' }
                dir('quiz-service') { sh 'mvn clean package -DskipTests' }
            }
        }

        stage('Deploy Infrastructure & Microservices') {
            steps {
                dir('infra') {
                    sh 'docker compose down'
                    sh 'docker compose up --build -d'
                }
            }
        }
    }
}