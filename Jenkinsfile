pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/karthik-anand/spring-petclinic.git'
      }
    }

    stage('Build & SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh './gradlew clean build sonarqube'
        }
      }
    }
  }

  post {
    success {
      echo 'Build and SonarQube analysis completed successfully.'
    }
    failure {
      echo 'Build or analysis failed.'
    }
  }
}