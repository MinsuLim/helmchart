def GIT_COMMIT_ID = script.sh(returnStdout: true, script: "git rev-parse --verify HEAD").trim()

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
            git credentialsId: 'github-neo-gmail',
                url: 'https://github.com/MinsuLim/helmchart.git',
                branch: 'main'

      //ssh -oStrictHostKeyChecking=no github.com
                
              sh '''  
                yq e -i '.image_name="${GIT_COMMIT_ID}"' values.yaml
                git add .
                git commit -am "helm update"
              '''
      
      //sh "git remote set-url origin git@github.com:MinsuLim/helmchart.git"
      //sh "git ls-remote -h git@github.com:MinsuLim/helmchart.git HEAD"
      
      // deleted
      //sh "git ls-remote -h git@github.com:MinsuLim/helmchart.git HEAD"
//                 sh " "
//                 sh "git config remote.origin.url git@github.com:MinsuLim/helmchart.git"
//                 sh " "
                
      
            sshagent(credentials: ['neo-github']) {
                sh "ssh-keyscan github.com >> /etc/ssh/ssh_known_hosts"
                sh "git push -u origin main"
             }
        }
        post {
                failure {
                  echo 'helm Manifest Update failure !'
                }
                success {
                  echo 'helm Manifest Update success !'
                }
        }
     
//      steps {
//         script {
//            sh "echo 'start deploy' "
     
        
//            withCredentials([sshUserPrivateKey(credentialsId: 'neo-github', keyFileVariable: 'keyfile')]) {
//               sh "git clone https://github.com/MinsuLim/helmchart.git"
            
//               sh '''  
//                 yq e -i '.image_name=""' values.yaml
//                 git add .'
//                 git commit -am "helm update"'
//                 git push origin main
//                 '''
//            }
//         }
//      }



    }
 }
}
