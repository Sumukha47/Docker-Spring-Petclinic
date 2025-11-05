pipeline {
    agent any

    environment {
        APP_NAME = "spring-petclinic"
    }

    tools {
        jdk 'java-home'
        maven 'maven-home'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sumukha47/spring-petclinic.git'
            }
        }

        stage('Build and Package') {
            steps {
                bat 'mvn clean package'
            }
        }
    }

    post {
        success {
            echo "Build succeeded for ${APP_NAME}"
            mail(
                to: 'sumukhadaring@gmail.com',
                subject: "SUCCESS: ${APP_NAME} Build #${env.BUILD_NUMBER}",
                body: "The Jenkins pipeline for ${APP_NAME} completed successfully.\nBuild URL: ${env.BUILD_URL}"
            )
        }

        failure {
            echo "Build failed for ${APP_NAME}"
            mail(
                to: 'sumukhadaring@gmail.com',
                subject: "FAILURE: ${APP_NAME} Build #${env.BUILD_NUMBER}",
                body: "The Jenkins pipeline for ${APP_NAME} has failed.\nPlease check console logs: ${env.BUILD_URL}console"
            )
        }
    }
}
