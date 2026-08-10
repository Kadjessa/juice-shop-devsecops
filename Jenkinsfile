pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Code récupéré depuis Git'
            }
        }

        stage('SAST') {
            steps {
                echo 'Analyse SAST'
            }
        }

        stage('Report') {
            steps {
                echo 'Génération du rapport'
            }
        }
    }
}
