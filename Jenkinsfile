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
                """
            }
        }
        stage('Unit test') {
            steps {
                script {
                    def result = sh(script: 'npm test', returnStatus: true)
                    if (result != 0) {
                        error "Unit tests failed"
                    } else {
                        echo "Unit tests passed"
                    }
                }
            }
        }
    }
}