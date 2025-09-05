pipeline {
    agent any
    tools {
        maven 'Maven-3.9'
        jdk 'Temurin-17'
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
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=jenkins_sonarqub'
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

