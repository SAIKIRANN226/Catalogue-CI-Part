pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment { 
        packageVersion = ''
        nexusURL = '172.31.5.95:8081'
    }
    options {
        ansiColor('xterm')
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'HOURS')
    }
    
    stages {
        stage('Get the Version'){
            steps{
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "Package.json version is: $packageVersion"
                }
            }
        }
        stage('Installing dependencies'){
            steps{
                sh """
                    npm install
                """
            }
        }
        stage('Unit Test'){
            steps {
                echo 'Running unit tests'
            }
        }
        stage('Sonar scan'){
            steps{
                script{
                    echo 'Running scan'
                }
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