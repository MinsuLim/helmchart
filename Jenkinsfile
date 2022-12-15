pipeline {
 
  agent { node { label 'ecs-agent-fargate' } } 
  stages { 

    //init stage 시작
    stage("init"){
      sh "echo init"
    }

    //build stage 시작
    stage("build"){
      sh "echo 'start build' "
    }
 
    //deploy stage 시작
    stage("deploy"){
      sh "echo 'start deploy' "

      withCredentials([gitUsernamePassword(credentialsId: 'neo-github', gitToolName: 'git-tool')]) {
        sh '''
          git clone 
          yq e -i '.image_name="test"' value.yaml
          git add .'
          git commit -am "helm update"'
          git push origin master
        '''
      }
    }
 }
}
