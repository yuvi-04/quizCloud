pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Compile & Package Microservices') {
            steps {
                sh '''
                    docker run --rm \
                      -v "${WORKSPACE}":/usr/src/app \
                      -v jenkins-maven-cache:/root/.m2 \
                      -w /usr/src/app \
                      maven:3.9.6-eclipse-temurin-17 \
                      bash -c "
                        cd config-server && mvn clean package -DskipTests && cd .. && \
                        cd service-registry && mvn clean package -DskipTests && cd .. && \
                        cd zipkin-server && mvn clean package -DskipTests && cd .. && \
                        api-gateway && mvn clean package -DskipTests && cd .. && \
                        cd question-service && mvn clean package -DskipTests && cd .. && \
                        cd quiz-service && mvn clean package -DskipTests
                      "
                '''
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