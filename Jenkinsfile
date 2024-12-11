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
                    echo "Unit testing is done"
                }
            }
        }
        stage('Build') {
            steps {
                sh """
                    ls -la
                """
            }
        }
    }
}