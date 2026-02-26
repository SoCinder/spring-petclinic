pipeline {
    agent any

    triggers {
        // Trigger every 5 minutes on Thursday
        cron('H/5 * * * 4')
    }

    tools {
        maven 'Maven3'   
        jdk 'JDK17'      
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SoCinder/spring-petclinic.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Code Coverage') {
            steps {
                // Generate Jacoco report
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    // Publish HTML Jacoco report in Jenkins
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: 'Jacoco Coverage Report'
                    ])
                }
            }
        }

        stage('Package Artifact') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}