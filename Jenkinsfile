pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    stages {
        stage('Clonar Repositório') {
            steps {
                sh 'git clone https://github.com/jasonsilvaa/HUB-DE-LEITURA.git'
            }
        }
        stage('Instalar dependências') {
            steps {
                dir('HUB-DE-LEITURA') {
                    sh 'npm install'
                }
                sh 'npm install'
            }
        }
        stage('Executar testes') {
            steps {
                sh '''
                    cd HUB-DE-LEITURA
                    nohup npm start > ../app.log 2>&1 &
                    cd ..
                    sleep 15
                    npm test
                '''
            }
        }
    }
}
