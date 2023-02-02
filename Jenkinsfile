pipeline {  
   agent any    
 tools {    
     // Install the Maven version configured as "M3" and add it to the path.  
       maven "M3"
    }
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository  
               git 'https://github.com/SubramanyaShastri/dev-project1.git' 
                 dir("C:/ProgramData/Jenkins/.jenkins/workspace/GenaralDevopsASsignment1_main")
  {        
          bat 'mvn -Dmaven.test.failure.ignore=true clean package'         
        }                    // Run Maven on a Unix agent.        
         bat "mvn -Dmaven.test.failure.ignore=true clean package"        
              // To run Maven on a Windows agent, use             
    //bat "mvn -Dmaven.test.failure.ignore=true clean package"       
      }        
 } 
    }   
   post {          
       // If Maven was able to run the tests, even if some of the test   
              // failed, record the test results and archive the jar file.   
       always {    
         junit(       
          allowEmptyResults: true,      
           testResults: '*/test-reports/.xml'      
     )      
    } 
       
       
       stage('Sonarqube Analysis') {
            steps {
                  withSonarQubeEnv('Server-sonar') {
           bat "mvn sonar:sonar \
                              -Dsonar.projectKey=maven-jenkins-pipeline \
                        -Dsonar.host.url=http://localhost:9000"
                }
           timeout(time: 2, unit: 'MINUTES') {
                      script {
                        waitForQualityGate abortPipeline: true
                    }
                }
              }
        }
 } 
}
