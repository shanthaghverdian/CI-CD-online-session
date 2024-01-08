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

    stage(''unit-test) {
   steps {
     script {
	   docker.image("${registry}":${env.Build_ID}").inside{
	     c-> sh 'python app_test.py' }
   }
   }
   }
    stage('Publish') {
      steps {
        script {
          docker.withRegistry('','dockerhub_user_pass'){
            docker.image("${registry}:${env.BUILD_ID}").push('latest')
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
