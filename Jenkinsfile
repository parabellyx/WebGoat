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
                sh './mvnw clean verify sonar:sonar -Dsonar.host.url=$SONAR_URL -Dsonar.projectKey=$SONAR_PROJECT -Dsonar.login=$SONAR_TOKEN'
            }
        }

        stage('dependency check'){
            steps{
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE'){
                        sh 'mvn org.owasp:dependency-check-maven:check'
                    }
            }
            post {
                always {
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'target/dependency-check-report.html', fingerprint: true, onlyIfSuccessful: true
                }
            }
        }

        stage('dependency tracker'){
            steps {
                sh 'mvn org.cyclonedx:cyclonedx-maven-plugin:makeAggregateBom'
            }
            post {
                success{
                    withCredentials([string(credentialsId: 'dt api-key', variable: 'API_KEY')]){
                        dependencyTrackPublisher artifact: 'target/bom.xml', projectName: 'WebGoat', projectVersion: '8', synchronous: true, dependencyTrackApiKey: API_KEY, projectProperties: [tags: ['UAT', 'Java'], swidTagId: 'my swid tag', group: 'Vuln1']
                    }
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'target/bom.xml', fingerprint: true, onlyIfSuccessful: true   
                }
            }
        }
    }

}