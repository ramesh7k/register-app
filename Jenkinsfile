pipeline{
	agent { label 'jenkins-slave' }
	tools {
		jdk 'Java17'
		maven 'Maven3'
                git 'Git_2.39'
	}
	stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
	        echo "Building ${env.JOB_NAME}..."
                }
        }
       stage("Checkout from SCM"){
                steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/ramesh7k/register-app'
                }
        }
      stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
     stage("SonarQube Analysis"){
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'jenkins-sonarqube') { 
                        sh "mvn sonar:sonar"
		        }
	           }	
           }
       }
    stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube'
                }	
            }

        }

	
}
}
