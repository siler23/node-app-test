pipeline {
  def registry = 'wsc-ibp-icp-cluster.icp:8500'  
  def namespace = 'walmart-lab'
  def app = 'node-web-app-wsc'
  def arch = 'amd64'
  def imageName = "${registry}/${namespace}/${app}-${arch}"
  def credentialLabel = 'team00'
  def customImage = ''
  def podlabel = "${app}-${UUID.randomUUID().toString()}"

  agent { 
    kubernetes {
      label podlabel
    }
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