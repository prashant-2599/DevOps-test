pipeline {
    agent {
        docker {
            image 'node:18-alpine' // Specify the Docker image for the build environment
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Docker socket binding
        }
    }

    environment {
        DOCKER_IMAGE = 'myapp' // Docker image name
        DOCKER_TAG = 'latest' // Docker tag
        REPO_URL = 'https://github.com/your-username/your-repo.git'
        DEPLOY_SERVER = 'user@your_server_ip'
        DOCKER_COMPOSE_PATH = '/path/to/docker-compose.yml'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                git url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to local registry...'
                // Here you can push to Docker Hub or any registry if required
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying application...'
                sh """
                ssh ${DEPLOY_SERVER} '
                docker pull ${DOCKER_IMAGE}:${DOCKER_TAG} &&
                docker-compose -f ${DOCKER_COMPOSE_PATH} up -d --force-recreate'
                """
            }
        }
    }
}
