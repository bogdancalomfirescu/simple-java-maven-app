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
      run_tests {
        sh 'mvn -B -DproxySet=true -DproxyHost=10.0.2.2 -DproxyPort=3128 clean test'
      }
      archive_results {
        archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
        junit 'build/reports/**/*.xml'
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
