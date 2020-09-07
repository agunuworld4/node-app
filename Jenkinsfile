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
    

     }
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}

