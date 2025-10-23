pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t flask-app .'
      }
    }

    stage('Test') {
      steps {
        sh 'echo "No tests defined."'
      }
    }

    stage('Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
          sh 'docker tag flask-app $DOCKER_USER/flask-app:latest'
          sh 'docker push $DOCKER_USER/flask-app:latest'
        }
      }
    }

    stage('Deploy') {
      steps {
        sh 'helm upgrade --install flask-app ./helm-chart'
      }
    }
  }
}
