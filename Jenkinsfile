pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('SCM') {
           steps {
             
                git branch: 'main', url: 'https://github.com/u16052642/PG-DO-DevOps-Certification-Project.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn clean install package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
               // sh 'docker tag samplewebapp softbayx/samplewebapp:latest'
                sh 'docker tag samplewebapp softbayx/samplewebapp:${BUILD_NUMBER}'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        //withDockerRegistry([ credentialsId: "Docker_hub_password", url: "" ]) {
          //sh  'docker push softbayx/samplewebapp:latest'
        //  sh  'docker push softbayx/samplewebapp:$BUILD_NUMBER' 
        withCredentials([string(credentialsId: 'Docker_hub_password', variable: 'VAR_FOR_DOCKERPASS')]) {
                    sh "sudo docker login -u softbayx -p $VAR_FOR_DOCKERPASS"
                    }
                    sh "sudo docker push softbayx/samplewebapp:${BUILD_NUMBER}"
        }
                  
          }
        
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
			    sh "sudo docker rm -f samplewebapp"
                sh "docker run -d -p 8083:8080 --name samplewebapp softbayx/samplewebapp:${BUILD_NUMBER}"
 
            }
        }
 //stage('Run Docker container on remote hosts') {
             
   //         steps {
    //            sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 softbayx/samplewebapp"
 
      //      }
        //}
    }
	}
