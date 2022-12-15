pipeline {
 
  agent { node { label 'ecs-agent-fargate' } } 
  stages { 

    //init stage 시작
    stage("init"){
     steps {
        script {
          sh "echo init"
        }
     }
    }

    //build stage 시작
    stage("build"){
     steps {
        script {
          sh "echo build"
        }
     }
    }
 
    //deploy stage 시작
    stage("deploy"){
     steps {
        script {
           sh "echo 'start deploy' "
        
           withCredentials([sshUserPrivateKey(credentialsId: 'neo-github', keyFileVariable: 'keyfile')]) {
              sh '''
                git clone https://github.com/MinsuLim/helmchart.git
                yq e -i '.image_name="test"' value.yaml
                git add .'
                git commit -am "helm update"'
                git push origin master
                '''
           }
        }
     }



    }
 }
}
