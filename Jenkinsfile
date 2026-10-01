pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Tests unitaires') {
            steps {
                dir('backend') {
                    sh 'mvn test'
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar',
                    fingerprint: true
            }
        }
    }

    post {
        failure {
            emailext(
                subject: "Échec du build Jenkins - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """Le build Jenkins a échoué.

Job : ${env.JOB_NAME}
Build : #${env.BUILD_NUMBER}
URL : ${env.BUILD_URL}

Consultez la console Jenkins pour voir les détails de l'erreur.
""",
                to: "ammar.ranim02@gmail.com"
            )
        }
    }
}
