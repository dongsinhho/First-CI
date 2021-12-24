pipeline {
    agent any
    stages {
       stage('compile') {
          steps {
             updateGitlabCommitStatus name: 'build', state: 'pending'
             bat 'gradlew.bat clean'
             echo 'Compile'
             bat 'gradlew.bat compileDebugSources'
          }
       }
       stage('unit test') {
          steps {
             echo 'Unit test'
             bat 'gradlew.bat test'
          }
       }
       stage('instrucment test') {
          steps {
             echo 'Instrucment test'
             bat 'C:/Users/dekintern/AppData/Local/Android/Sdk/platform-tools/adb devices -l'
             bat 'gradlew.bat cAT'
          }
       }
       stage('build apk') {
          steps {
             echo 'Build file APK'
             bat 'gradlew assembleDebug'
          }
       }
    }
   post {
        always {
            echo 'Save APK file'
            archiveArtifacts(allowEmptyArchive: true, artifacts:'app/build/outputs/apk/debug/*.apk')
        }
        success {  
            echo 'Send Email SUCCESSFUL via Outlook'
            mail bcc: '', body: "<b>SUCCESSFUL CI</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "SUCCESSFUL CI: Project name -> ${env.JOB_NAME}", to: "ext.sinh.nd.ho@dektech.com.au";  
            updateGitlabCommitStatus name: 'build', state: 'success'
        }
	     failure { 
           echo 'Send Email ERROR via Outlook' 
	        mail bcc: '', body: "<b>ERROR CI</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "ext.sinh.nd.ho@dektech.com.au";  
            updateGitlabCommitStatus name: 'build', state: 'failed'
        } 
    }
}
