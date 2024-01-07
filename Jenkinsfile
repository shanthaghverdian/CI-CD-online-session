pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:${env.Build_ID}")
        }

      }
    }

    stage('Publish') {
      steps {
        script {
          docker.withRegistry('','dockerhub_user_pass'){
            docker.image("{$registry}):${env.Build_ID}").push ('latest')
            docker.image("${registry}:${env.BUILD_ID}").push("${env.BUILD_ID}")
          }
        }

      }
    }

  }
  environment {
    registry = 'shanthaghverdian/test'
  }
}