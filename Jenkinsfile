pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/karthik-anand/spring-petclinic.git'
      }
    }

    // stage('Build') {
    //   steps {
    //     echo 'Building the app with Maven with skiptests...'
    //     sh './mvnw clean install -DskipTests'
    //   }
    // }
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