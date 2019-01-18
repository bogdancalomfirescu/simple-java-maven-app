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
          withMaven(maven: 'Maven 3.5') {
            sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 clean package sonar:sonar'
          }

        }

      }
    }
    stage('Quality Gate') {
      steps {
        timeout(time: 1, unit: 'HOURS') {
          waitForQualityGate true
        }

      }
    }
  }
}