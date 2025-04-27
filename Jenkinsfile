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
        echo 'Building the app with Maven with skiptests...'
        sh './mvnw clean install -DskipTests'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarScanner') {
          sh '''
                ./mvnw sonar:sonar \
                -DskipTests \
                -Dsonar.inclusions=src/main/java/org/springframework/samples/petclinic/owner/**
            '''
        }
      }
    }

    stage('Deploy App on prod-server') {
      steps {
          dir('ansible') {
              sh 'ansible-playbook -v -i hosts playbook.yml'
          }
      }
    }

    stage('OWASP ZAP Scan') {
      steps {
        sh '''
          mkdir -p zap-output
          docker run --rm -u zap \
            -v $(pwd)/zap-output:/zap/wrk \
            ghcr.io/zaproxy/zaproxy:stable \
            zap.sh -cmd \
              -quickurl http://172.16.119.128:8080/ \
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