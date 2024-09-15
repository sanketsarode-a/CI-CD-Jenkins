pipeline {
  agent any

  environment {
    DOCKER_REGISTRY_CREDS = 4e5f7cde-6a78-47b3-9449-afd240cb3d83 // Replace with actual credentials ID
  }

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t my-flask-app .'
        sh "docker tag my-flask-app $DOCKER_BFLASK_IMAGE"
      }
    }
    stage('Test') {
      steps {
        sh 'docker run my-flask-app python -m pytest app/tests/'
      }
    }
    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
          sh "docker push $DOCKER_BFLASK_IMAGE"
        }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
