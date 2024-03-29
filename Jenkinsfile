pipeline {

  environment {
    registry = 'wsc-ibp-icp-cluster.icp:8500'  
    namespace = 'walmart-lab'
    app = 'node-web-app-wsc'
    arch = 'amd64'
    imageName = "${registry}/${namespace}/${app}-${arch}"
    dockerID = '71fb157d-3d75-47f8-8109-6a99280637df'
    customImage = ''
  }

  agent { 
    kubernetes {
      label "default-jenkins-${UUID.randomUUID().toString()}"
    }
  }
 
  stages {

    stage ('Checkout source code from github repo') {
      steps {
        checkout scm
      }
    }

    stage ('Tests') {
      steps {
        sh "echo Running My Test Scripts"
        /*
          Would put tests here
        */
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
          withDockerRegistry([credentialsId: "${dockerID}", url: "https://${registry}"]){
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