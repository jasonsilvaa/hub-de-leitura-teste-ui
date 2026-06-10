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

                    echo "Aguardando aplicação em http://localhost:3000..."
                    for i in $(seq 1 60); do
                        if curl -s -o /dev/null http://localhost:3000/; then
                            echo "Aplicação disponível."
                            break
                        fi
                        if [ "$i" -eq 60 ]; then
                            echo "Timeout aguardando aplicação. Log:"
                            cat app.log
                            exit 1
                        fi
                        sleep 2
                    done

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
