pipeline {
    agent any 
    tools {
        maven 'M3'


    }
    stages {
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarexp') {
                    // Optionally use a Maven environment you've configured already
                 bat "mvn sonarexp \
                              -Dsonar.projectKey=maven-jenkins-pipeline \
                        -Dsonar.host.url=http://localhost:9000" 
                    withMaven(maven:'M3') {
                          dir("C:/ProgramData/Jenkins/.jenkins/workspace/GenaralDevopsASsignment1_main") {
                        bat 'mvn clean package sonar:sonar'
                          }
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
