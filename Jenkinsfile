pipeline {
    agent any

    environment {
        APP_NAME = "spring-petclinic"
        BUILD_DIR = "target"
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build from')
    }

    options {
        timeout(time: 15, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    tools {
        jdk 'java-home25'
        maven 'maven-home'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }

        stage('Build and Package') {
            steps {
                bat 'mvn package'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                echo "Artifact archived successfully from ${BUILD_DIR}"
            }
        }
    }

    post {
        success {
            echo "Build succeeded for ${APP_NAME}"
            mail(
                bcc: '',
                cc: 'akshathabm20@gmail.com',
                from: 'sumukhadaring@gmail.com',
                replyTo: '',
                subject: "SUCCESS: ${APP_NAME} Build #${env.BUILD_NUMBER}",
                to: 'sumukhadaring@gmail.com',
                body: """Hello Developer,

This is to inform you that the Jenkins pipeline for ${env.JOB_NAME} has completed successfully.

Build Details:
- Job Name: ${env.JOB_NAME}
- Build Number: ${env.BUILD_NUMBER}
- Build Timestamp: ${env.BUILD_TIMESTAMP}
- Executed By: Jenkins CI
- Repository: https://github.com/Sumukha47/spring-petclinic.git

Build URL: ${env.BUILD_URL}
Artifacts: ${env.BUILD_URL}artifact/target/
Console Logs: ${env.BUILD_URL}console

All stages were executed successfully and the artifacts have been generated.

Best regards,
Jenkins CI Server
"""
            )
        }

        failure {
            echo "Build failed for ${APP_NAME}"
            mail(
                bcc: '',
                cc: 'akshathabm20@gmail.com',
                from: 'sumukhadaring@gmail.com',
                replyTo: '',
                subject: "FAILURE: ${APP_NAME} Build #${env.BUILD_NUMBER}",
                to: 'sumukhadaring@gmail.com',
                body: """Hello Developer,

This is to inform you that the Jenkins pipeline for ${env.JOB_NAME} has failed during execution.

Build Details:
- Job Name: ${env.JOB_NAME}
- Build Number: ${env.BUILD_NUMBER}
- Build Timestamp: ${env.BUILD_TIMESTAMP}
- Executed By: Jenkins CI
- Repository: https://github.com/Sumukha47/spring-petclinic.git

Build URL: ${env.BUILD_URL}
Artifacts (if any): ${env.BUILD_URL}artifact/target/
Console Logs: ${env.BUILD_URL}console

Please review the console output and error logs at the above link to identify and resolve the issue.

Best regards,
Jenkins CI Server
"""
            )
        }
    }
}
