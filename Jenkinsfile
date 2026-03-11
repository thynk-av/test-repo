pipeline {
    agent any

    environment {
        IMAGE_NAME = "test-app"
        CONTAINER_NAME = "test-app-container"
    }

    stages {

        stage('Install Python') {
            steps {
                sh '''
                apt-get update
                apt-get install -y python3 python3-pip
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install --break-system-packages -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true

                docker run -d -p 5000:5000 \
                --name $CONTAINER_NAME \
                $IMAGE_NAME
                '''
            }
        }

    }
}
