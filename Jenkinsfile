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
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/ramesh7k/maven'
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
	
}
}
