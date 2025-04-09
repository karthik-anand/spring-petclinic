pipeline {
   agent any
 
//    tools {
//      // Auto-install SonarQube scanner
//      sonarQubeScanner 'SonarScanner'
//    }
 
//    environment {
//      SONARQUBE_URL = 'http://sonarqube:9000'
//    }
 
   stages {
     stage('Checkout') {
       steps {
         git 'https://github.com/karthik-anand/spring-petclinic'
       }
     }
 
    //  stage('SonarQube Analysis') {
    //    steps {
    //      withSonarQubeEnv('SonarQube') {
    //        sh './mvnw clean verify sonar:sonar'
    //      }
    //    }
    //  }
 
     stage('Build') {
       steps {
         echo 'Building the app...'
         echo 'Build complete'
       }
     }
   }
}