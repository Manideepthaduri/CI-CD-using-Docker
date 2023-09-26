pipeline {
    agent any
	
	  tools
    {
       maven "Maven 3.8.7"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/Manideepthaduri/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp manideep9/samplewebapp:latest'
                //sh 'docker tag samplewebapp manideep9/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
      steps{
          withCredentials([usernamePassword(credentialsId: 'DockerHub_username_password', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
          sh '''
          docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD
          docker push manideep9/samplewebapp:$BUILD_NUMBER
	   '''

   }
            }

     
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 manideep9/samplewebapp"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@Mani run -d -p 8003:8080 manideep9/samplewebapp"
 
            }
        }
    }
	
    
