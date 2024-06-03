pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-flask-app:latest"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], 
                          userRemoteConfigs: [[url: 'https://github.com/iamironman4279/bk.git']]
                ])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -d -p 5000:5000 ${DOCKER_IMAGE}'
                }
            }
        }
    }

    post {
        always {
            script {
                try {
                    sh 'docker ps -a'
                } catch (Exception e) {
                    echo "Error in post stage: ${e.getMessage()}"
                }
            }
        }

        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
