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
    }
}