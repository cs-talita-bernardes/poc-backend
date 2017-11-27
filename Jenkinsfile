pipeline {
    agent any
    stages{
     
         stage( "Checkout"){
            steps{
                git url: 'https://github.com/teamconcrete/poc-backend.git', branch: 'develop'
            }

         }

         stage("Build"){
            steps{ 
              sh "gradle wrapper && gradle build --stacktrace"
              archiveArtifacts artifacts: '**', fingerprint: true
              
            }
         }
        
        stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
               echo 'Sucesso'
               slackSend channel: "#devops-onboarding", message: "Build Started: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
            }
            
        }
    }    

      
}
