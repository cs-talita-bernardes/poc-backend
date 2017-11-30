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
              sh "gradle clean assemble && gradle wrapper  && gradle build --stacktrace"
              archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
              
            }
         }
        
        stage ("Nexus Publish"){
            steps{ 
                sh 'curl --upload-file ${WORKSPACE}/build/libs/*.jar -u ${user}:${pass} -v http://nexus.csteam.tk/nexus/content/repositories/releases/artefact-${BUILD_NUMBER}'
            }     
        }
        
        
        
        stage('Send Message') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
               echo 'Sucesso'
               slackSend channel: "#devops-onboarding",color: '#0000FF', message: "Build Started: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
            }
            
        }
    }    

      
}
