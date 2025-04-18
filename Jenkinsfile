pipeline {
  agent any

  environment {
    NGROK_URL = 'https://0968-128-237-82-112.ngrok-free.app' // Replace with your actual ngrok URL
  }


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

    // stage('SonarQube Analysis') {
    //   steps {
    //     withSonarQubeEnv('SonarScanner') {
    //       sh '''
    //             ./mvnw sonar:sonar \
    //             -DskipTests \
    //             -Dsonar.inclusions=src/main/java/org/springframework/samples/petclinic/owner/**
    //         '''
    //     }
    //   }
    // }

    stage('OWASP ZAP Scan') {
      steps {
        sh '''
          mkdir -p zap-output
          docker run --rm -u zap \
            -v $(pwd)/zap-output:/zap/wrk \
            ghcr.io/zaproxy/zaproxy:stable \
            zap.sh -cmd \
              -quickurl $NGROK_URL \
              -quickout /zap/wrk/report.html \
              -quickprogress
        '''
      }
    }

  }

  post {
    always {
      archiveArtifacts artifacts: 'zap-output/report.html', allowEmptyArchive: true

      publishHTML(target: [
        reportName: 'OWASP ZAP Report',
        reportDir: 'zap-output',
        reportFiles: 'report.html',
        keepAll: true,
        alwaysLinkToLastBuild: true,
        allowMissing: true
      ])
    }
    success {
      echo 'Build and tests completed successfully!'
    }
    failure {
      echo 'Build or tests failed. Check console output.'
    }
  }
}