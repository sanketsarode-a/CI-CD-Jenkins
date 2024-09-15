pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = 'docker.io/sanket821/my-flask-app:latest'
        DOCKER_USERNAME = 'sanket821'
        DOCKER_PASSWORD = 'SARODE@123'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE_NAME} .'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'docker run --rm ${DOCKER_IMAGE_NAME} python -m pytest app/tests/'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh "docker push ${DOCKER_IMAGE_NAME}"
                }
            }
        }
    }
    post {
        always {
            script {
                sh 'docker logout'
            }
        }
    }
}
