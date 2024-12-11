pipeline {
    agent {
        node {
            label 'SAIKIRAN-LABEL'
        }
    }

    stages {
        stage('Get the version') {
            steps {
                script {
                    env.Version=readJSON(file: 'package.json').version
                    echo "Package version is $version"
                }
            }
        }
        stage('Installing dependencies') {
            steps {
                sh """
                    npm install
                    echo "Dependencies installed"
                    npm install -g pm2
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
                    sshPublisher(publishers: [sshPublisherDesc(configName: 'producction', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''sudo apt install unzip -y
                    unzip catalogue.zip''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/home/centos/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                }
            }
        }
    }
}