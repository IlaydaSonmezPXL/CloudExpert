pipeline {
    agent {
        docker {
            image 'node:22'
        }
    }

    environment {
        CI = 'true'
        API_TOKEN = credentials('sample-api-token')
    }

    stages {
        stage('Dependencies') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm ci'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Security Check') {
            steps {
                sh '''
                    if [ -n "$API_TOKEN" ]; then
                        echo "API token was injected successfully."
                    else
                        echo "API token was not injected."
                        exit 1
                    fi
                '''
            }
        }
    }

    post {
        always {
            cleanWs deleteDirs: true, notFailBuild: true
        }

        failure {
            echo 'Pipeline failed. Check build logs for failure diagnostics.'
        }
    }
}
