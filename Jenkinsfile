import groovy.json.JsonOutput
import groovy.transform.Field
import groovy.json.JsonSlurper

node ('Build-Server') 
{

      parameters 
    {
           string(name: 'Service_Name', defaultValue: 'account-service', description: 'Which service needs to deploy')
           choice(choices: 'develop\nrelease\nfeature\nmaster', description: 'Select the Branch Name' , name: 'Branch_Name')
           choice(choices: 'Dev\nPreProd\nProd', description: 'Select the runtime environment', name: 'Server_Environment')
    }
    

    
     
      
        if ("${BRANCH_NAME}" == 'master')
       {
         
            Checkout()   //  cloning 
            Build()      // mvn clean install
            Create_Image() //docker build 
            docker_image_push() // for pushing the image 
        
            
    
      }
}



def Checkout() 
{
    stage('Checkout')
    {
    
        checkout scm
        
    }
    
}
def Build() 
{
    stage('Build') 
    {
       sh "cd ${params.Service_Name} ; mvn clean install " 
    }
}

def Create_Image()
{
    stage('Create_Image') 
    {
        sh "cd $WORKSPACE/${params.Service_Name} ; sudo docker build --tag=${params.Service_Name} ."
    }
}
def docker_image_push()
{
      stage('image pushing')
      {
            sh "sudo docker login -uyourusername -pyourpasword"
            sh " sudo docker tag ${params.Service_Name} ankit1111/${params.Service_Name}:v1"
            sh " sudo docker push ankit1111/${params.Service_Name}:v1"
            
      }
} 
