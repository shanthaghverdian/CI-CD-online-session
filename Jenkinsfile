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

    stage('unit-test') {
      steps {
        script {
	   docker.image("${registry}:${env.Build_ID}").inside{
	     c-> sh 'python app_test.py' }
   }
   }
   }

	  stage('http-test'){
      steps{
        script{
          docker.image("${registry}:${env.BUILD_ID}").withRun('-p 9005:9000'){
            c -> sh "sleep 5; curl -i http://localhost:9006/test_string"
          }
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
