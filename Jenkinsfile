pipeline{
	agent { label 'jenkins-slave' }
	tools {
		jdk 'Java17'
		maven 'Maven3'
                git 'Git_2.39'
	}
	environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "ramesh7k"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
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
    stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

       }

	
}
}
