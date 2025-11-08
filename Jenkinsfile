pipeline {
    agent any
    tools {
        maven 'maven'
    }

    environment {
        // Nexus details
        NEXUS_URL = "http://nexus:8081/repository/maven-releases/"
        NEXUS_CREDENTIALS_ID = "nexus-cred-id"  // Jenkins credentials ID for Nexus
        GROUP_ID = "com.boardgame"
        ARTIFACT_ID = "boardgame-app"
        VERSION = "1.0.${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Checking out code from GitHub..."
                checkout scm
            }
        }

        stage('Build Artifact using Maven') {
            tools { maven 'maven' }
            steps {
                configFileProvider([configFile(fileId: '6f6fe0af-c937-440b-98f5-af73ab3f1358', variable: 'mavensettings')]) {
                    echo "Building project with Maven..."
                    sh 'mvn -s $mavensettings clean deploy -DskipTests=true'
                 }
                
                
            }
        }

        
        stage('Post Build Cleanup') {
            steps {
                echo "Cleaning up workspace..."
                cleanWs()
            }
        }
    }

    post {
        success {
            echo "✅ Build and artifact upload successful!"
        }
        failure {
            echo "❌ Build or upload failed!"
        }
    }
}
