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
        stage('Nexus artifact uploader') {
            steps {
                script {
                    NEXUS_URL="http://35.173.231.255:8081/repository/catalogue/"
                    REPO="catalogue"
                    FILE_PATH="catalogue.zip" 
                    GROUP_ID="com.roboshop" 
                    ARTIFACT_ID="catalogue" 
                    VERSION="1.0.0"  
                    UPLOAD_URL="${NEXUS_URL}/repository/${REPO}/${GROUP_ID}/${ARTIFACT_ID}/${VERSION}/${ARTIFACT_ID}-${VERSION}.zip"
                    NEXUS_USERNAME="admin"
                    NEXUS_PASSWORD="saikiran123"
                }
            }
        }
    }
}