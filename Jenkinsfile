pipeline {
    agent any
    stages {
        stage('SCM') {
            steps {
                git url: 'https://github.com/jenkins-docs/simple-java-maven-app.git'
            }
        }
        stage('SonarQube analysis') {
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
                    // Requires SonarQube Scanner for Jenkins 2.7+
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
