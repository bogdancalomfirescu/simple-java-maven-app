pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('build && SonarQube analysis') {
      steps {
        withSonarQubeEnv('My SonarQube Server') {
          // Optionally use a Maven environment you've configured already
          withMaven(maven:'Maven 3.5') {
            sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 clean package sonar:sonar'
          }
        }
      }
    }
    stage("Quality Gate") {
      steps {
        timeout(time: 1, unit: 'HOURS') {
          // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
          // true = set pipeline to UNSTABLE, false = don't
          waitForQualityGate abortPipeline: true
        }
      }
    }   
    stage('Maven Build ') {
      parallel {
        stage('Maven Build ') {
          steps {
            sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 -DskipTests clean package'
          }
        }
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          post {
            always {
              junit 'target/surefire-reports/*.xml'

            }

          }
          steps {
            sh 'mvn -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 test'
          }
        }
        stage('Archive Artifacts') {
          steps {
            archiveArtifacts 'target/*.jar'
          }
        }
      }
    }
    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
      }
    }
  }
}
