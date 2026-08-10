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
                    . || true
                '''
            }
        }

        stage('Check Report') {
            steps {
                sh 'ls -lh semgrep-report.json'
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
            emailext(
                subject: "Rapport SAST Juice Shop - Build #${BUILD_NUMBER}",
                body: """
Bonjour,

Le pipeline SAST de Juice Shop est terminé.

Build : #${BUILD_NUMBER}
Résultat : ${currentBuild.currentResult}

Le rapport Semgrep est joint à cet email.

Jenkins :
${BUILD_URL}
""",
                to: "connectdiass@gmail.com",
                attachmentsPattern: "semgrep-report.json"
            )
        }
    }
}
