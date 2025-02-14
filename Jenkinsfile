pipeline {
    agent any
    tools {
        nodejs 'nodejs-23.4' // Ensure this tool name matches the Node.js installation configured in Jenkins
    }
    environment {
        MONGO_URI = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
    }

    stages {
        stage('Installing Dependencies') {
            steps {
                sh 'npm install --no-audit'
            }
        }

        stage('Security Checks') {
            parallel {
                stage('NPM Dependency Audits') {
                    steps {
                        sh '''
                            npm audit --audit-level=critical
                            if [ $? -ne 0 ]; then
                                echo "Critical vulnerabilities found!"
                                exit 1
                            fi
                        '''
                    }
                }

                stage('OWASP Dependencies Check') {
                    steps {
                        dependencyCheck additionalArguments: '''
                            --scan ./
                            --out ./
                            --format ALL
                            --prettyPrint
                        ''', odcInstallation: 'OWAS-DepCheck-12' // Ensure this matches the OWASP Dependency-Check installation name in Jenkins

                        dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: true

                        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: './', reportFiles: 'dependency-check-jenkins.html', reportName: 'Dependency Check HTML Report', reportTitles: '', useWrapperFileDirectly: true])

                        junit allowEmptyResults: true, stdioRetention: '', testResults: 'dependency-check-junit.xml'
                    }
                }
            }
        }

        stage('Unit Testing') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'mongo', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
                    sh 'npm test'
                }
                junit allowEmptyResults: true, stdioRetention: '', testResults: 'test-result.xml'
            }
        }
    }
}
