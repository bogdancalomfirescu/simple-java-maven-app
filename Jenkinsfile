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
        withSonarQubeEnv('My SonarQube Server') {
          sh 'mvn -X -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 -DskipTests -Djavax.net.ssl.trustStore=/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts -Djavax.net.ssl.trustStorePassword=changeit sonar:sonar'
        }

      }
    }
  }
}