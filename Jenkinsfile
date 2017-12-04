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
                withCredentials([usernamePassword(credentialsId: '7d13ef7c-195a-44f2-bd62-95ba137ffcc2', passwordVariable: 'NX_PASS', usernameVariable: 'NX_USER')]) {
                    sh "curl --upload-file ${WORKSPACE}/build/libs/*.jar -u $NX_USER:$NX_PASS -v http://nexus.csteam.tk/nexus/content/repositories/releases/artefact-${BUILD_NUMBER}.jar"
                }
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
