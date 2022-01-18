pipeline {
  agent any
  environment 
  {
    btime= "${BUILD_TIMESTAMP}"
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {

    stage('checkout') 
    {
      steps
      {
        slackSend (color: '#00ff04', message: "Build is triggered ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL})")
        checkout scm
      }
    }
  
    stage('Scan') {
      steps {
        withSonarQubeEnv(installationName: 'first-project-sq1') { 
          sh './mvnw clean sonar:sonar'
        }
      }
    }
    stage("Quality Gate") {
      steps {
        timeout(time: 3, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }

  }
}
