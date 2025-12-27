pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQube' // Name configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sachinashokyadav/boardgame-java.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
