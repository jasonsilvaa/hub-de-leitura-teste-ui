pipeline {
    agent any
    tools {
        nodejs 'Node'
    }
    stages {
        stage('Clonar Repositório') {
            steps {
                sh '''
                    if [ -d hub-de-leitura-integrado/.git ]; then
                        git -C hub-de-leitura-integrado pull
                    else
                        rm -rf hub-de-leitura-integrado
                        git clone https://github.com/jasonsilvaa/hub-de-leitura-integrado.git
                    fi
                '''
            }
        }
        stage('Instalar dependências') {
            steps {
                dir('hub-de-leitura-integrado') {
                    sh 'npm install'
                }
                sh 'npm install'
            }
        }
        stage('Executar testes') {
            steps {
                sh '''
                    cd hub-de-leitura-integrado
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
    }
}
