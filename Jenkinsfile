pipeline {
    agent any
    environment {
        NETWORK_NAME = "polo_net"
        CONTAINER_NAME = "DB_PoloIT"
    }
    stages {
        stage('Verify docker') {
            steps {
                script {
                    sh "docker rm -f ${CONTAINER_NAME} || echo 'El contenedor no existe.'"
                    sh "docker network ls --filter name=^${NETWORK_NAME} -q | grep -q . || docker network create -d bridge ${NETWORK_NAME}"
                }
            }
        }
        stage('Run PostgreSQL Container') {
            steps {
                script {
                    echo 'Iniciando el contenedor de PostgreSQL...'
                    sh """
                    docker compose up -d
                    """
                }
            }
        }
        stage('Verify PostgreSQL Container') {
            steps {
                script {
                    echo 'Verificando si el contenedor está corriendo...'
                    sh 'docker ps | grep ${CONTAINER_NAME}'
                }
            }
        }
    }
    post {
        always {
            script {
                echo 'Finalizando el pipeline...'
            }
        }
    }
}
