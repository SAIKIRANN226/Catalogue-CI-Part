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
                    zip -rq archive.zip Jenkinsfile package.json package-lock.json server.js schema .git node_modules
                    mv archive.zip catalogue.zip
                    echo "Zip files success"
                """
            }
        }
    }
}