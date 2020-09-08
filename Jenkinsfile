pipeline {
     agent any

     environment{
          DOCKER_TAG = getDockerTag()
     }

     stages {
         stage( 'Build Docker Image'){
             steps{
                 sh "docker build . -t agunuworld/nodeapp:${DOCKER_TAG} "
             }
         }
      
      stage('Push Docker Image'){
          steps{
              withCredentials([string(credentialsId: 'dockerAuthenticationpublic', variable: 'dockerAuthenticationpublic')])  {
              sh "docker login -u agunuworld -p ${dockerAuthenticationpublic}"
           }
          sh "docker push agunuworld/nodeapp:${DOCKER_TAG} "
          }
          }
       
       stage('Deploy To Kubernetes'){
           steps{
               sh "chmod +x changeTag.sh"
               sh "./changeTag.sh ${DOCKER_TAG}"
               sshagent(['eksclimasternodes']) {
               sh "scp -o StrictHostKeyChecking=no service.yml node-app-pod.yml ec2-user@3.135.209.242:/home/ec2-user/"
               script{
                   try{
                       sh "ssh ec2-user@3.135.209.242 kubectl apply -f ."
                   }catch(error){
                       sh "ssh ec2-user@3.135.209.242 kubectl create -f ."
                   }
               }
            }
           }
       }
      
     }
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}

