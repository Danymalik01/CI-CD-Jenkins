pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Build the code using Maven
                sh 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                // Run unit tests using JUnit
                sh 'mvn test'
                // Run integration tests using Selenium WebDriver
                sh 'mvn integration-test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Integrate a code analysis tool to analyse the code using Jenkins
                jenkinsCodeAnalysis tool: 'SonarQube'
            }
        }
        stage('Security Scan') {
            steps {
                // Perform a security scan on the code using OWASP ZAP
                sh 'docker run --rm owasp/zap2docker-stable zap-baseline.py -t http://localhost:8080'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Deploy the application to a staging server using SSH
                sshPublisher(
                    continueOnError: false, failOnError: true, publishers: [
                        sshPublisherDesc(
                            configName: 'StagingServer',
                            verbose: true,
                            transfers: [
                                sshTransfer(
                                    sourceFiles: 'target/myapp.war',
                                    removePrefix: 'target/',
                                    remoteDirectory: '/var/www/myapp'
                                )
                            ]
                        )
                    ]
                )
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Run integration tests on the staging environment using Selenium WebDriver
                sh 'mvn integration-test -Dwebdriver.chrome.driver=/path/to/chromedriver'
            }
        }
        stage('Deploy to Production') {
            steps {
                // Deploy the application to a production server using SSH
                sshPublisher(
                    continueOnError: false, failOnError: true, publishers: [
                        sshPublisherDesc(
                            configName: 'ProductionServer',
                            verbose: true,
                            transfers: [
                                sshTransfer(
                                    sourceFiles: 'target/myapp.war',
                                    removePrefix: 'target/',
                                    remoteDirectory: '/var/www/myapp'
                                )
                            ]
                        )
                    ]
                )
            }
        }
    }
}
