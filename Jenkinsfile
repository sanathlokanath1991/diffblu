pipeline {
    agent any

    tools {
        maven 'Maven 3.8.1'  // Replace with your Maven installation name in Jenkins
        diffblueCover 'DiffblueCoverCLI' // Replace with your Diffblue Cover CLI tool name
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git(credentialsId: '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', // Replace with your credentials ID
                    url: 'https://github.com/sanathlokanath1991/diffblu.git',     // Replace with your repo URL
                    branch: 'main')                       // Replace with your branch
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
                publishHTML([allowMissing: true,  // Changed to true to avoid build failure if the report is not generated
                             alwaysLinkToLastBuild: true,
                             keepAll: false,
                             reportDir: 'coverage-report', // Corrected to the actual directory name
                             reportFiles: 'index.html',
                             reportName: 'Diffblue Coverage Report',
                             title: 'Diffblue Coverage Report'])
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
