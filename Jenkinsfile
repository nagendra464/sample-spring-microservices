import groovy.json.JsonOutput
import groovy.transform.Field
import groovy.json.JsonSlurper

node ('Build-Server') 
{
 environment{
      TAG = "${BUILD_ID}"
    }
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
          sh "cd $WORKSPACE/${params.Service_Name} ; sudo docker build --tag=${params.Service_Name}:${TAG} ."
    }
}
def docker_image_push()
{
      stage('image pushing')
      {
            withCredentials([string(credentialsId: 'docker-hub', variable: 'docker-hub')]) {
                  sh "sudo docker login -u nagendra464 -p ${docker-hub}"
      }
            
            sh " sudo docker tag ${params.Service_Name} nagendra464/${params.Service_Name}:${TAG}"
            sh " sudo docker push nagendra464/${params.Service_Name}:${TAG}"
            
      }
} 
