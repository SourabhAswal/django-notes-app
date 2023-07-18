pipeline{
    
    agent any
    
    stages{
        
       stage("Github Code"){
           steps{
               echo"Code from GITHUB"
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: 
               [[credentialsId: 'github', url: 'https://github.com/SourabhAswal/django-notes-app.git']])
               
           }
           
       }
       
       
       stage("Build Image "){
           
           steps{
               echo"Building docker images"
               sh "docker build -t my-notes-app ."
               
           }
           
       }
       
       stage("Push Image in Docker hub"){
           
           steps{
               
               
         withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubpassword', usernameVariable: 'dockehubuser')]) {
    
        sh "docker tag my-notes-app ${env.dockehubuser}/my-notes-app:latest"
        sh "docker login -u ${env.dockehubuser} -p ${env.dockerhubpassword}" 
        sh "docker push ${env.dockehubuser}/my-notes-app:latest"
            
             
         }
               echo"Push docker images in Dockerhub"
               
           }
           
           
       }
       
       stage("Deployed in  EC2"){
           
           steps{
               echo"Deployed code in EC2"
               
               sh "docker-compose down && docker-compose up -d "
               
               }
           
       }
       
       
       
    }
}
