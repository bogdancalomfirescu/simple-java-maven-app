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
//        withSonarQubeEnv('My SonarQube Server') {
//         sh 'mvn -X -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 -DskipTests sonar:sonar'
//        }
          sh 'echo Hello World'
      }
    }
    stage('JUnit testing') {
      steps {
          sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 clean test'
//          sh 'echo Hello World'
      }
    }    
  }
}
