pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
        sh "docker build -t ppazdziorek/people:${env.BUILD_NUMBER} ."
      }
    }    
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'passwordVariable.', usernameVariable: 'usernameVariable')]) {
          sh "docker login -u ppazdziorek -p Leibnitz1."
          sh "docker push ppazdziorek/people:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Docker Remove Local Image') {
      steps {
        sh "docker rmi ppazdziorek/people:${env.BUILD_NUMBER}"
      }
    }    
    stage('Apply Kubernetes Manifest') {
      steps {                     
        sh 'cat k8s/all-in-one.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'                  
      }
    }
  }
}