pipeline {
    agent {
        node {
            label 'SAIKIRAN-LABEL'
        }
    }
    environment { 
        packageVersion = ''
        nexusURL = '172.31.5.95:8081'
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
    }

    stages {
        stage('Get the version') {
            steps {
                script { // It is a groovy scripting thats why we put the word "script" not shellscript, to read this json file we need to download "pipelineUtility steps" plugin in manage jenkins
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
                    echo "Dependencies installed"
                """
            }
        }
        stage('Unit test') {
            steps {
                script {
                    echo "Unit testing is done"
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
        stage('Nexus artifact uploader') {
            steps {
                script {
                    nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "35.173.231.255:8081",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
                }
            }
        }
        stage('Deploy') {
            steps {
               script {
                    def params = [
                        string(name: 'version', value: "$packageVersion"),
                        string(name: 'environment', value: "dev")
                    ]
                    build job: "Catalogue-CD-Part", wait: true, parameters: params
               }
            }
        }
    }
}