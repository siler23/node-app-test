pipeline {

  agent any

  environment {
    registry = 'https://wsc-ibp-icp-cluster.icp:8500'  
    namespace = 'walmart-lab'
    image = 'node-web-app'
    arch = 's390x'
    imageName = "${registry}/${namespace}/${image}-${arch}"
    credentialLabel = 'docker'
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
          docker.withRegistry('https://wsc-ibp-icp-cluster.icp:8500', 'docker')
          customImage = docker.build("${image}:${env.BUILD_ID}")
          customImage.push()
          customImage.push('latest')
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
           /*docker.withRegistry(registry, credentialLabel)
            customImage = 
            customImage.push()
            customImage.push('latest')*/
          sh "echo 'today is the day'"
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