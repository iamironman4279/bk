pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_IMAGE = 'my-flask-app:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository
                git branch: 'main', url: 'https://github.com/iamironman4279/bk.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                // Run the Docker container
                script {
                    docker.image(env.DOCKER_IMAGE).run('-p 5000:5000')
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker containers and images
            script {
                def containers = docker ps -a -q
                if (containers) {
                    sh 'docker rm -f $(docker ps -a -q)'
                }

                def images = docker images -q
                if (images) {
                    sh 'docker rmi -f $(docker images -q)'
                }
            }
        }
    }
}
