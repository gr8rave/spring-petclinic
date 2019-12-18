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
        
                 
        stage ('static code analysis'){
			steps {
				dir ("$(SRC_DIR){
				withSonarQubeEnv ('localsonar'){
			bat "sonar-scanner -Dsonar.host.url=http://localhost:9090"
			}
		script {
			def qualitygate= waitForQualityGate()
				if (qualitygate.status != "Pass") {
				error "Pipeline abborted"
				
        }
		}
		}
		}
		}	
    }
}
