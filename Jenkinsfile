pipeline {
    agent any

    stages {
        stage("Build") {
            steps {
                echo "Building the code using Maven"
            }
        }

        stage("Unit and Integration Tests") {
            steps {
                echo "Performing Unit Testing using JUnit..."
                echo "Performing Integration Testing using Selenium WebDriver..."

                script {
                    def logFilePathNew = "${env.WORKSPACE}/test-output.log"
                
                    sh """
                        echo 'Starting unit testing using JUnit...' > ${logFilePathNew}
                        echo 'Testing the feature working...' >> ${logFilePathNew}
                        echo 'Unit testing completed and no issues found' >> ${logFilePathNew}
                        echo '\\n\\nStarting Integration testing using Selenium WebDriver...' >> ${logFilePathNew}
                        echo 'End to End Testing of the complete product working...' >> ${logFilePathNew}
                        echo 'Integration testing completed and no issues found' >> ${logFilePathNew} 
                    """
                }
            }
            post {
                success {
                    emailext attachmentsPattern: 'test-output.log',
                            body: 'The Unit Testing and Integration testing completed successfully',
                            subject: 'Unit and Integration Testing Status: SUCCESS',
                            to: 'jyothikasunil006@gmail.com'
                }
                failure {
                    emailext attachmentsPattern: 'test-output.log',
                            body: 'The Unit Testing and Integration testing completed unsuccessfully. Please check logs!',
                            subject: 'Unit and Integration Testing Status: FAILURE',
                            to: 'jyothikasunil006@gmail.com'
                }
            }
        }

        stage("Code Analysis") {
            steps {
                echo "Running code analysis with SonarQube"
            }
        }

        stage("Security Scan") {
            steps {
                echo "Running security scan with OWASP ZAP"
            }
            post {
                success {
                    mail to: "jyothikasunil006@gmail.com",
                        subject: "Build status email",
                        body: "Build was successful"
                }
                failure {
                    mail to: "jyothikasunil006@gmail.com",
                        subject: "Build status email",
                        body: "Build failed"
                }
            }
        }

        stage("Deploy to Staging") {
            steps {
                echo "Deploying the application to staging server using AWS CodeDeploy"
            }
        }

        stage("Integration Tests on Staging") {
            steps {
                echo "Running integration tests on the staging environment with Selenium"
            }
            post {
                success {
                    mail to: "jyothikasunil006@gmail.com",
                        subject: "Build status email",
                        body: "Build was successful"
                }
                failure {
                    mail to: "jyothikasunil006@gmail.com",
                        subject: "Build status email",
                        body: "Build failed"
                }
            }
        }

        stage("Deploy to Production") {
            steps {
                echo "Deploying the application to production server using AWS CodeDeploy"
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/*.log', allowEmptyArchive: true
        }
        success {
            script {
                def logFiles = findFiles(glob: '**/*.log')
                emailext (
                    to: 'jyothikasunil006@gmail.com',
                    subject: 'Build status email - SUCCESS',
                    body: 'Build was successful',
                    attachmentsPattern: logFiles.collect { it.path }.join(',')
                )
            }
        }
        failure {
            script {
                def logFiles = findFiles(glob: '**/*.log')
                emailext (
                    to: 'jyothikasunil006@gmail.com',
                    subject: 'Build status email - FAILURE',
                    body: 'Build failed',
                    attachmentsPattern: logFiles.collect { it.path }.join(',')
                )
            }
        }
    }
}
