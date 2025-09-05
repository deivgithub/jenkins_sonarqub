pipeline {
    agent any
    tools {
        maven 'Maven-3.9'
        jdk 'Temurin-17'
    }

    environment {
        SONAR_AUTH_TOKEN = credentials('sonar-token') // credencial en Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/deivgithub/jenkins_sonarqub.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                 withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh """
                    mvn sonar:sonar \
                      -Dsonar.projectKey=jenkins_sonarqub \
                      -Dsonar.host.url=http://172.27.205.177:9000 \
                      -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}

