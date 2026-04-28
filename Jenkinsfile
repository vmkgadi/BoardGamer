pipeline {
    agent {
    docker {
        image 'maven:3.9.9-eclipse-temurin-17'
    }
}
    environment {
        IMAGE_NAME = 'myapp'
        TAG = "${BUILD_NUMBER}"
        PORT = '8082'
    }

    tools {
        jdk 'jdk17'   // configure this in Jenikins
        maven 'maven3'
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building with Maven...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container...'
                sh '''
                docker stop $IMAGE_NAME || true
                docker rm $IMAGE_NAME || true
                docker run -d -p $PORT:8080 --name $IMAGE_NAME $IMAGE_NAME:$TAG
                '''
            }
        }

        stage('Health Check') {
            steps {
                echo 'Checking application health...'
                sh 'sleep 10'
                sh 'curl -f http://localhost:$PORT/actuator/health'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

