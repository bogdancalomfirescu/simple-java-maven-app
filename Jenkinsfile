pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('SonarQube Analysis') {
          withSonarQubeEnv('My SonarQube Server') {
            // requires SonarQube Scanner for Maven 3.2+
            sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
          }
        }      
        stage('Build') {
            steps {
                sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
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
