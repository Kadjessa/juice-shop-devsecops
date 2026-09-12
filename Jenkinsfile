pipeline {
agent any

```
stages {

    stage('Checkout') {
        steps {
            checkout scm
        }
    }

    stage('Build / Preparation') {
        steps {
            sh '''
                echo "Préparation du projet pour l'analyse de sécurité"
                node --version
                npm --version
            '''
        }
    }

    stage('Security Analysis - Semgrep') {
        steps {
            sh '''
                mkdir -p reports

                /opt/semgrep-venv/bin/semgrep scan \
                    --config p/javascript \
                    --json \
                    --output reports/semgrep.json \
                    .
            '''
        }
    }

    stage('Additional Security Check - Gitleaks') {
        steps {
            sh '''
                mkdir -p reports

                /usr/local/bin/gitleaks detect \
                    --source . \
                    --report-format json \
                    --report-path reports/gitleaks.json \
                    --exit-code 0
            '''
        }
    }

    stage('Report Generation') {
        steps {
            archiveArtifacts artifacts: 'reports/*.json',
                allowEmptyArchive: true
        }
    }

    stage('Notification') {
        steps {
            echo "Analyses de sécurité terminées."
        }
    }
}

post {
    always {
        echo "Pipeline terminé avec le statut : ${currentBuild.currentResult}"
    }
}
```

}
