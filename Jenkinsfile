pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }

    environment {
        packageVersion = ''
        nexusURL = "52.90.5.129:8081"
    }

    options {
        ansiColor('xterm')
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'HOURS')
    }

    stages {
        stage('Get the version') {
            steps {
               script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version: $packageVersion"
                }
            }
        }
        stage('Installing dependencies') {
            steps {
                sh """
                    npm install
                """
            }
        }
        stage('Unit test') {
            steps {
                sh """
                    echo "unit tests will run here"
                """
            }
        }
        stage('Sonar Scan'){
            steps{
                sh """
                    echo "sonar-scanner"
                """
            }
        }
        stage('Build') {
            steps {
                sh """
                    ls -la 
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                """
            }
        }
    }
}