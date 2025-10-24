pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Build stage started'
        sh 'docker build -t flask-app .'
      }
    }

    stage('Test') {
      steps {
        echo 'Test stage started'
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
        echo 'Deploy stage started'
        sh 'helm upgrade --install flask-app ./helm-chart'
      }
    }
  }
}
