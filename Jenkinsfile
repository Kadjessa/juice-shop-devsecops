pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Code Juice Shop récupéré'
            }
        }

        stage('SAST') {
            steps {
                sh '''
                    /opt/semgrep-venv/bin/semgrep scan \
                    --config=auto \
                    --json \
                    --output=semgrep-report.json \
                    .
                '''
            }
        }

        stage('Archive Report') {
            steps {
                archiveArtifacts artifacts: 'semgrep-report.json',
                                 fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé'
        }
    }
}
