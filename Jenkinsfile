pipeline {
  agent any
  tools {nodejs "node"}
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Ahmad20/myapp'
      }
    }
    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Build Docker Image'){
      agent any
      steps{
        sh 'docker build -t ahmadtrg/myapp:1.0.0 .'
      }
    }
    stage('Push Docker Image'){
      steps{
        withCredentials([string(credentialsId: 'pass_dockerhub', variable: 'dockerpass')]) {
          sh "docker login -u ahmadtrg -p ${dockerpass}"
        }
        sh 'docker push ahmadtrg/myapp:1.0.0'
      }
    }
    stage('Run Container on Localhost'){
      steps{
        sh 'docker rm -f myapp'
        sh 'docker run -itd --name myapp -p 8181:80 ahmadtrg/myapp:1.0.0'
      }
    }
  }
}
