pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('Maven Build ') {
      steps {
        sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 -DskipTests clean package'
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