pipeline {
    agent {
        node {
            label 'AGENT SAIKIRAN'
        }
    }
    environment {
        packageVersion = ''
        nexusURL = ''
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
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
        stage('Install dependencies') { // For building the code we need npm dependencies
            steps {
                sh """
                    npm install
                """
            }
        }
    }
}