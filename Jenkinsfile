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
          echo "BUILD_NUMBER $BUILD_NUMBER"
          echo "BUILD_ID $BUILD_ID"
          echo "JOB_NAME $JOB_NAME"
          echo "JOB_BASE_NAME $JOB_BASE_NAME"
          echo "BUILD_TAG $BUILD_TAG"
          echo "EXECUTOR_NUMBER $EXECUTOR_NUMBER"
          echo "NODE_NAME $NODE_NAME"
          echo "NODE_LABELS $NODE_LABELS"
          echo "WORKSPACE $WORKSPACE"
          echo "JENKINS_HOME $JENKINS_HOME"
          echo "JENKINS_URL $JENKINS_URL"
          echo "BUILD_URL $BUILD_URL"
          echo "JOB_URL $JOB_URL"
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
          steps {
            sh "docker rmi ${customImage}"
          }
        }
    }
}