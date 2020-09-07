pipeline {
     agent any

     environment{
          DOCKER_TAG = getDockerTag()
     }

     stages {
         stage( 'Build Docker Image'){
             steps{
                 sh "docker build -t agunuworld/nodeapp:v1 ."
             }
         }

    

     }
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}

