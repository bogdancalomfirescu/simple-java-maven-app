pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('SonarQube analysis') {
      steps {
        sh 'echo Hello World'
      }
    }
    stage('JUnit testing') {
      steps {
        script {
          try {
            sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 surefire:test'
          }
          catch(err) {
            step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
            if (currentBuild.result == 'UNSTABLE')
            currentBuild.result = 'FAILURE'
            throw err
          }
        }

      }
    }
  }
}