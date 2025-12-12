pipeline {
  agent any
  environment {
    // EXAMPLE: expose credentials if needed (credentials id defined in Jenkins)
    // MY_SECRET = credentials('my-secret-id')
  }
  options {
    skipDefaultCheckout(true)
    // timeout(time: 30, unit: 'MINUTES')
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh './gradlew clean build' // or mvn package, or npm install && npm test
      }
    }
    stage('Test') {
      steps {
        sh './gradlew test'
        junit '**/build/test-results/**/*.xml'
      }
    }
    stage('Publish') {
      when { branch 'main' } // run only on main
      steps {
        echo 'Publish artifacts...'
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
    }
    success {
      echo 'Success'
    }
    failure {
      mail to: 'team@example.com', subject: "Build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}", body: "Check Jenkins"
    }
  }
}
