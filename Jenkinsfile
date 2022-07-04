pipeline {
    agent any

    tools {
        maven 'maven 3.8.6'
    }

    environment {
        SONAR_URL       = credentials('SONAR_URL')
        SONAR_PROJECT   = credentials('SONAR_PROJECT')
        SONAR_TOKEN     = credentials('SONAR_TOKEN')
    }

    stages {
        
        stage('SonarQube Analysis'){
            steps {
                sh "./mvnw clean verify sonar:sonar -Dsonar.host.url=$SONAR_URL -Dsonar.projectKey=$SONAR_PROJECT -Dsonar.login=$SONAR_TOKEN"
            }
        }
    }

}