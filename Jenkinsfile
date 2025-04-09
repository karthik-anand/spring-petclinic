pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/karthik-anand/spring-petclinic.git'
      }
    }

    stage('Build') {
      steps {
        echo 'Building the app with Maven...'
        sh './mvnw clean install'
      }
    }
  }

  post {
    success {
      echo 'Build and tests completed successfully!'
    }
    failure {
      echo 'Build or tests failed. Check console output.'
    }
  }
}