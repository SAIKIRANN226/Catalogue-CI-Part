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
    }
    
    stages {
        stage('Get the Version'){
            steps{
                script {
                    def packageJson = readJSON file: 'package.json'
                    echo "Package.json version is: $packageVersion"
                }
            }
        }
    }
}