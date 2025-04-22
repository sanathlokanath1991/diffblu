pipeline {
    agent any

    tools {
        maven 'Maven3.9.0'  //  Corrected tool name.  Must match EXACTLY what's in Jenkins.
        // diffblueCover 'DiffblueCoverCLI' // Remove or comment out if not a standard Jenkins tool
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from GitHub') {
            steps {
                git(credentialsId: 'github-credentials',
                    url: 'https://github.com/sanathlokanath1991/diffblu.git',
                    branch: 'main')
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Run Diffblue Cover') {
            steps {
                sh 'dcover create --project . --junit-report junit-report.xml'
            }
        }
        stage('Publish Coverage Report') {
            steps {
                publishHTML(
                    [
                        allowMissing: true,
                        alwaysLinkToLastBuild: true,
                        keepAll: false,
                        reportDir: 'coverage-report',
                        reportFiles: 'index.html',
                        reportName: 'Diffblue Coverage Report',
                        title: 'Diffblue Coverage Report',
                    ]
                )
            }
        }
        stage('Publish JUnit Report') {
            steps {
                junit 'junit-report.xml'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
