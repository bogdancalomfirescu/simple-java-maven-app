pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 -DskipTests clean package'
      }
    }
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
    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
      }
    }
  }
  environment {
    http_proxy = 'http://10.0.2.2:3128/'
  }
}