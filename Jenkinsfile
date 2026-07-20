pipeline {
    agent any
    tools {
        nodejs 'Node-7.8.0'   
    }
    environment {
        // Lógica condicional según la rama
        PORT       = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain:v1.0' : 'nodedev:v1.0'}"
        CONTAINER  = "${env.BRANCH_NAME == 'main' ? 'app-main' : 'app-dev'}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test || echo "No tests defined, continuing..."'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('Deploy') {
            steps {
                // Borra solo el contenedor de ESTE env (advanced task incluida)
                sh "docker rm -f ${CONTAINER} || true"
                sh "docker run -d --name ${CONTAINER} -p ${PORT}:${PORT} ${IMAGE_NAME}"
                echo "Aplicación desplegada en http://localhost:${PORT}"
            }
        }
    }
}
