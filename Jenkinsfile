pipeline {
    agent any

    environment {
        IMAGE_NAME = "spring-helloworld:latest"  // Docker image name
        CONTAINER_NAME = "spring-helloworld"    // Docker container name
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/syedimran18/spring-helloworld', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                # Stop and remove existing container if it exists
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true

                # Run the Docker container
                docker run -d --name $CONTAINER_NAME -p 8080:8081 $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo 'Application successfully built and deployed!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
        always {
            cleanWs() // Clean up workspace
        }
    }
}
