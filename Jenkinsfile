pipeline {
    agent any

    stages {

        stage('Clean Reports') {
            steps {
                sh '''
                    rm -rf test-reports || true
                    mkdir -p test-reports
                '''
            }
        }

        stage('Build Stage') {
            steps {
                echo 'Installing dependencies'
                sh '''
                    python -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Testing Stage') {
            steps {
                echo '********* Test Stage Started **********'
                sh '''
                    . venv/bin/activate
                    pytest test.py --junitxml=test-reports/results.xml
                '''
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
                    sh '''
                        . venv/bin/activate
                        python app.py
                    '''
                }
                echo '********* Deploy Stage Finished **********'
            }
        }
    }

    post {
        always {
            echo 'We came to an end!'
            junit allowEmptyResults: true, testResults: 'test-reports/results.xml'
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
