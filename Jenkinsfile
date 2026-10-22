pipeline {
  agent any
  environment {
    dockerhub = credentials('dockerhub')
    SECRET_VAR = credentials('secret_text')
  }

  stages {
    stage('Reset') {
      steps {
        sh "docker compose -f compose.yaml down --rmi=all"
      }
    }
    stage('Build') {
      steps {
        sh "docker compose -f compose.yaml build"
      }
    }
    stage('Push') {
      steps {
        sh "docker login -u ${dockerhub_USR} -p ${dockerhub_PSW}"
        sh "docker compose -f compose.yaml push"
        sh "docker logout"
      }
    }
    stage('Deploy') {
      steps {
        sh "htpasswd -bc .htpasswd admin ${SECRET_VAR}"
        sh "docker compose -f compose.yaml up -d"
      }
    }
  }
}
