pipeline {
    agent any
    tools {
        maven 'Maven-3.9'
        jdk 'Temurin-17'
    }

    stages {
        stage('Checkout') {
            steps {
		git(
		    url: 'https://github.com/deivgithub/jenkins_sonarqub.git',
		    branch: 'main',
		    credentialsId: 'github-token',
		)
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
                    sh 'mvn sonar:sonar -Dsonar.projectKey=jenkins_sonarqub -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml'
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

