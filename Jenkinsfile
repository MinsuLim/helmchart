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

              sh '''  
                yq e -i '.image_name="test"' value.yaml
                git add .'
                git commit -am "helm update"'
                '''
            sshagent(credentials: ['neo-github']) {
                sh "git remote set-url origin git@github.com:MinsuLim/helmchart.git"
                sh "git push -u origin master"
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
//                 yq e -i '.image_name="test"' value.yaml
//                 git add .'
//                 git commit -am "helm update"'
//                 git push origin master
//                 '''
//            }
//         }
//      }



    }
 }
}
