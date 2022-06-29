node {

  environment {
    SONAR_URL = credentials('SONAR_URL')
    TOKEN     = credentials('TOKEN')
  }

  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'maven 3.8.6';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=webgoat -Dsonar.host.url=$SONAR_URL -Dsonar.login=$TOKEN"
    }
  }
}