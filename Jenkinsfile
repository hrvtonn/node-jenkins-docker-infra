pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build / Instalação') {
            steps {
                sh 'npm install'
            }
        }

        stage('SAST (Segurança)') {
            steps {
                sh 'npm audit --audit-level=high'
            }
        }

        stage('Lint & Quality') {
            steps {
                sh 'npx eslint src/ --env node --env es2021'
            }
        }

        stage('Testes') {
            steps {
                sh 'npm test'
            }
        }
    }

    post {
        always {
            deleteDir()
        }
        success {
            echo 'Pipeline executada com sucesso em todos os estágios!'
        }
        failure {
            echo 'A pipeline falhou em algum estágio. Verifique os logs.'
        }
    }
}
