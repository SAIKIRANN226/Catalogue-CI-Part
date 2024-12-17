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
                script { // It is a groovy scripting thats why we put the word "script" not shellscript
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
                    zip -rq archive.zip Jenkinsfile package.json package-lock.json server.js schema .git node_modules
                    mv archive.zip catalogue.zip
                    echo "Zip files success"
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
                    version: "${version}",
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