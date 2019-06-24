pipeline {
    environment {
      registry = 'https://wsc-ibp-icp-cluster.icp:8500'  
      namespace = 'walmart-lab'
      image = 'node-web-app'
      arch = 's390x'
      imageName = "${registry}/${namespace}/${image}-${arch}"
      customImage = ""
      credentialLabel = 'docker'
    }
    agent any
 
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
            docker.withRegistry(registry, credentialLabel)
            customImage.push()
            customImage.push('latest')
          }
        }
      }

        stage('Remove Docker Image After Push') {
           sh "docker rmi ${customImage}"
        }
    }
}