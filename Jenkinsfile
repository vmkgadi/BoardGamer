pipeline
{
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean package -DskipTests' // Add your build commands here
            }
        }
        stage("Docker Build") {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t myapp:latest .' // Add your Docker build commands here}
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                sh '''
                docker run -d -p 8082:8080 myapp:latest
                ''' // Add your deploy commands here
            }
        }
    }
}