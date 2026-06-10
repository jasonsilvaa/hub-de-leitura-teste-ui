pipeline {
    agent any
    tools {
        nodejs 'Node'
    }
    stages {
        stage('Clonar Repositório') {
            steps {
                sh '''
                    if [ -d HUB-DE-LEITURA/.git ]; then
                        git -C HUB-DE-LEITURA pull
                    else
                        rm -rf HUB-DE-LEITURA
                        git clone https://github.com/jasonsilvaa/HUB-DE-LEITURA.git
                    fi
                '''
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
    post {
        always {
            sh 'pkill -f "node" || true'
        }
        failure {
            emailext(
                to: 'jason.silva@ebac.com.br',
                subject: 'Falha no pipeline',
                body: 'O pipeline falhou. Por favor, verifique o log para mais detalhes.'
            )
        }
        success {
            emailext(
                to: 'jason.silva@ebac.com.br',
                subject: 'Pipeline executado com sucesso',
                body: 'O pipeline executou com sucesso. Por favor, verifique o log para mais detalhes.'
            )
        }
    }
}
