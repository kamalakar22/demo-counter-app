pipeline{
    
    agent any 
   
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', credentialsId: '10376970-92a5-4331-a1cb-20432b66cb7d',
                        url: 'https://github.com/kamalakar22/demo-counter-app.git'
                }
            }
        }
       
       
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh "mvn clean install"
                }
            }
        }
       
           
        }
        
}
