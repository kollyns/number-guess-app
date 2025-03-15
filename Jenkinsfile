pipeline {
    agent any

    tools {
        // Use the Maven tool defined in Jenkins Global Tool Configuration
        maven "Maven"
    }

    environment {
        // Optional: you can define JAVA_HOME or other env vars here
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kollyns/number-guess-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive WAR Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh 'scp -i ~/Ceeyit-class.pem target/*.war ec2-user@3.15.140.90:/opt/tomcat/webapps/'
            }
        }
    }

    post {
        success {
            echo "✅ Build, Test and Deployment completed successfully!"
        }
        failure {
            echo "❌ Build failed. Please check logs."
        }
    }
}