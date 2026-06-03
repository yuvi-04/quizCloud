pipeline {
    agent any

    tools {
        maven 'maven-3.9.9'
    }

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
            steps {
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
                    withDockerContainer(image: 'docker:dind', args: '-v /var/run/docker.sock:/var/run/docker.sock') {
                        sh 'docker compose down || true'
                        sh 'docker compose up --build -d'
                    }
                }
            }
        }
    }
}