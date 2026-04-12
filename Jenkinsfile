pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                python -m venv venv
                venv\\Scripts\\activate
                pip install -r requirements.txt
                pip install pytest pytest-html pytest-cov
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                venv\\Scripts\\activate
                pytest test.py --junitxml=results.xml
                '''
            }
            post {
                always {
                    junit 'results.xml'
                }
            }
        }
    }
}