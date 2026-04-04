pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/harinip1509/hotel-management-devops.git'
        BRANCH_NAME = 'main'

        IMAGE_NAME = 'hotel-app'
        CONTAINER_NAME = 'hotel-container'
        PORT = '8085'
    }

    stages {

        stage('Cleanup') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: "${BRANCH_NAME}", url: "${REPO_URL}"
            }
        }

        stage('Build (Maven)') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        // 🔽 Docker starts AFTER CI/CD steps
        stage('Build Docker Image') {
            steps {
                sh '/usr/local/bin/docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                /usr/local/bin/docker rm -f $CONTAINER_NAME || true
                /usr/local/bin/docker run -d -p $PORT:8080 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
