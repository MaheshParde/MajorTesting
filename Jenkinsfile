pipeline {
    
	  agent any
  tools {nodejs "node"}

	environment{
	registry="maheshparde/nodejs"
	registryCredential='dockerhub'
	dockerImage=''
	}
	stages{
	stage('Git') {
		steps{
		git 'https://github.com/MaheshParde/MajorTesting'
		}	
	}
	
	stage('Code Analysis') {
      steps {
        script {
          scannerHome = tool 'sonarqube'
        }
        withSonarQubeEnv('sonarqube') {
          git 'https://github.com/MaheshParde/MajorTesting',
          sh  "mvn -Dspring.profiles.acitve=dev -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml clean verify sonar:sonar"

        }
      }
    }
    
	
		
		
	stage('Building image') {
		steps{
			script{
			 	dockerImage=docker.build registry	
			}
		}
	}
	stage('Registring image') {
		steps{
			script{
				docker.withRegistry('',registryCredential){
				dockerImage.push()
				}
			}
		}
	}
	}
    
}
