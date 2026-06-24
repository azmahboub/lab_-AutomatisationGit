pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/azmahboub/lab_-AutomatisationGit.git',
                    credentialsId: 'github-Token'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("app-web:${env.BUILD_ID}")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker stop app-web-demo || true'
                    sh 'docker rm app-web-demo || true'
                    dockerImage.run("-d -p 8083:80 --name app-web-demo")
                }
            }
        }

        stage('🧪 Test') {
            steps {
                sh 'sleep 2'
                sh 'curl --fail http://host.docker.internal:8083'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline réussi ! Application accessible sur http://localhost:8083'
        }
        failure {
            echo '❌ Échec du pipeline. Consultez les logs.'
        }
    }
}
