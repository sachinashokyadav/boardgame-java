pipeline {
    agent any

    // environment {
    //     SONARQUBE = 'SonarQube' // must match Jenkins SonarQube install name
    // }

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Checkout') {
            steps {
                // ensures correct branch
                git branch: 'feature-sachin', url: 'https://github.com/sachinashokyadav/boardgame-java.git'
                sh "pwd"
                sh "ls -lrt"
            }
        }

        stage('Build') {
            steps {
                // compile and install locally
                sh 'mvn clean install -DskipTests'
                sh "pwd"
                sh "ls -lrt"
                sh "cd target"
                sh "ls -la"
            }
        }

        // stage('SonarQube Analysis') {
        //     steps {
        //         withSonarQubeEnv('SonarQube') {
        //             // invoke sonar plugin explicitly
        //             sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=boardgame-java -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_AUTH_TOKEN'
        //         }
        //     }
        // }

        // stage('Quality Gate') {
        //     steps {
        //         timeout(time: 5, unit: 'MINUTES') {
        //             // waits for SonarQube quality gate status
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }

    }
}
