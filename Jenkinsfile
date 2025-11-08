pipeline {
    agent any
    tools {
        maven 'maven'
    }

    environment {
        // Nexus details
        NEXUS_URL = "http://localhost:8081/repository/maven-releases/"
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
                echo "Building project with Maven..."
                sh 'mvn clean package -DskipTests=true'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                echo "Uploading artifact to Nexus Repository..."
                script {
                    def artifactPath = "target/${ARTIFACT_ID}-${VERSION}.jar"

                    sh """
                    mv target/*.jar ${artifactPath}

                    mvn deploy:deploy-file \
                        -DgroupId=${GROUP_ID} \
                        -DartifactId=${ARTIFACT_ID} \
                        -Dversion=${VERSION} \
                        -Dpackaging=jar \
                        -Dfile=${artifactPath} \
                        -DrepositoryId=nexus \
                        -Durl=${NEXUS_URL} \
                        -DgeneratePom=true \
                        --settings ~/.m2/settings.xml
                    """
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
