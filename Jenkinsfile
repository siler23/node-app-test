pipeline {

  agent any

  environment {
    registry = 'https://wsc-ibp-icp-cluster.icp:8500'  
    namespace = 'walmart-lab'
    image = 'node-web-app'
    arch = 's390x'
    imageName = "${registry}/${namespace}/${image}-${arch}"
    credentialLabel = 'docker'
    customImage=''
  }
 
  stages {

    stage ('Checkout source code from github repo') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          customImage = docker.build("${image}:${env.BUILD_ID}")
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          withDockerRegistry([credentialsId: "${credentialLabel}", url: "${registry}"]){
            customImage.push()
            customImage.push('latest')
          }
        }
      }
    }

      stage('Remove Docker Image After Push') {
        steps {
          sh "docker rmi ${customImage}"
        }
      }
    }
}