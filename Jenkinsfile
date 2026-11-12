pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {
        stage('Instalação das dependencias') {
            steps {
                echo 'Instalando os pacotes node...'
                sh 'npm install'
            }
        }

        stage('Execução dos testes') {
            steps {
              sh 'npm run teste-suite-1'
              sh 'npm run teste-suite-2'
              sh 'npm run teste-suite-3'
            }
        }

        stage('Gerar relatório Allure') {
            steps {
                echo '📊 Gerando relatório Allure...'
                // gera o relatório HTML
                sh 'npx allure generate allure-results --clean -o allure-report || true'
                // salva como artefato opcional
                archiveArtifacts artifacts: 'allure-report/**/*.*', allowEmptyArchive: true
            }
        }

        stage('Publicar relatório no Jenkins') {
            steps {
                echo '🌐 Publicando relatório Allure no Jenkins...'
                allure([
                    includeProperties: false,
                    jdk: '',
                    results: [[path: 'allure-results']]
                ])
            }
        }

    }

        post {
            success {
                echo 'Build e testes executados com sucesso'
            }
            failure {
                echo 'Falha na execução do pipeline'
            }
            always {
            archiveArtifacts artifacts: 'cypress/videos/**/*.*,cypress/screenshots/**/*.*', allowEmptyArchive: true
        }
    }




}