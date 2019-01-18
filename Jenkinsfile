pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git 'https://github.com/jenkins-docs/simple-java-maven-app.git'
      }
    }
    stage('SonarQube analysis') {
      steps {
        withSonarQubeEnv('My SonarQube Server') {
            sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 clean package sonar:sonar'

        }

      }
    }
    stage('Quality Gate') {
      steps {
        timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
          def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
          if (qg.status != 'OK') {
            error "Pipeline aborted due to quality gate failure: ${qg.status}"
          }
        }

      }
    }
  }
}
