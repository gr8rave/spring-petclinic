pipeline{
    agent any
    tools {
        maven 'maven3'
        jdk 'jdk8'
    }
    stages{
            stage("SCM"){
            steps{
                
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/NEWGEN-GIT/spring-petclinic']]])
            }
        }
        
        stage("Build"){
            steps{
                bat 'mvn clean package'
            }
        }
        

        stage("SCA"){
            steps{
                bat 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9090'
            }
        }
        
       stage("Quality Gate"){
          //timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
          def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
             if (qg.status != 'Passed') {
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }

        
  }
}

