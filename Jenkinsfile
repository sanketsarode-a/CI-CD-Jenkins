pipeline {
    agent any
    
    environment {
        // Define environment variables
        DOCKER_IMAGE_NAME = 'docker.io/sanket821/my-flask-app:latest'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t my-flask-app .'
                    // Tag Docker image
                    sh "docker tag my-flask-app ${env.DOCKER_IMAGE_NAME}"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run tests using Docker
                    sh 'docker run --rm my-flask-app python -m pytest app/tests/'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Docker login and push image
                    withCredentials([usernamePassword(credentialsId: 'docker-registry-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                        sh "docker push ${env.DOCKER_IMAGE_NAME}"
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Logout from Docker
                sh 'docker logout'
            }
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        unstable {
            echo 'Pipeline is unstable!'
        }
    }
}
