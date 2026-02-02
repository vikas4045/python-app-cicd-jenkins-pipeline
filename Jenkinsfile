pipeline {
    agent any

    stages {

        stage('Clean Reports') {
            steps {
                bat 'rmdir /s /q test-reports || exit 0'
            }
        }

        stage('Build Stage') {
            steps {
                echo 'Installing dependencies'
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Testing Stage') {
            steps {
                echo '********* Test Stage Started **********'
                bat 'python test.py'
                echo '********* Test Stage Finished **********'
            }
        }

        stage('Sanity Check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }

        stage('Deployment Stage') {
            steps {
                input "Do you want to Deploy the application?"
                echo '********* Deploy Stage Started **********'
                timeout(time: 1, unit: 'MINUTES') {
                    bat 'python app.py'
                }
                echo '********* Deploy Stage Finished **********'
            }
        }
    }

    post {
        always {
            echo 'We came to an end!'
            junit 'test-reports/*.xml'
            deleteDir()
        }

        success {
            echo 'Build Successful!!'
        }

        failure {
            echo 'Sorry mate! build has Failed :('
        }

        unstable {
            echo 'Run was marked as unstable'
        }

        changed {
            echo 'Pipeline state has changed.'
        }
    }
}
