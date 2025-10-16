pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/eyabrahmi/supraapp.git', branch: 'main'
      }
    }

    stage('Build image') {
      steps {
        sh 'docker build -t supra-app .'
      }
    }

    stage('Scan image (Trivy)') {
      steps {
        sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image --severity HIGH,CRITICAL --exit-code 0 supra-app || true'
      }
    }

    stage('Deploy locally') {
      steps {
        sh 'docker compose up -d --build'
      }
    }
  }
}
