pipeline {

    environment {
    registry = 'wsc-ibp-icp-cluster.icp:8500'  
    namespace = 'walmart-lab'
    app = 'node-web-app'
    arch = 'amd64'
    imageName = "${registry}/${namespace}/${app}-${arch}"
    credentialLabel = 'docker'
    customImage = ''
    label = "${app}-${UUID.randomUUID().toString()}"
  }
  
  agent { 
    kubernetes {
      label label
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
          customImage = docker.build("${imageName}:${env.BUILD_ID}")
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          withDockerRegistry([credentialsId: "${credentialLabel}", url: "https://${registry}"]){
            customImage.push()
            customImage.push('latest')
          }
        }
      }
    }

      stage('Remove Docker Image After Push') {
        steps {
          sh "docker rmi \$(docker images --format '{{.Repository}}:{{.Tag}}' | grep \"${imageName}\")"
        }
      }
    }
}