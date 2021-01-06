pipeline {
 agent any
 
 environment {
   
    PROJECT_NAME = "AppAsp2"
    PUBLISH_TARGET_PATH_PROD = "D:\\WWWROOT\\TestJenkins"

}

 stages {

  stage('Build') {
    stages {

        stage('deploy PROD') {
            when {
                branch 'master'
            }
            steps {
                echo 'Building PROD...'
                bat "\"C:/Program Files/dotnet/dotnet.exe\" restore \"${workspace}/${env.PROJECT_NAME}.csproj\""
                bat "\"C:/Program Files/dotnet/dotnet.exe\" build \"${workspace}/${env.PROJECT_NAME}.csproj\" --configuration Release"
                bat "\"C:/Program Files/dotnet/dotnet.exe\" publish \"${workspace}/${env.PROJECT_NAME}.csproj\" --configuration Release"
             
            }
        }
    }
  }

   stage('Test') {
        stages {
     
            stage('Test PROD') {
                when {
                    branch 'master'
                    expression {
                        currentBuild.result == null || currentBuild.result == 'SUCCESS' 
                      }
                }
                steps {
                    echo 'Test PROD...'
                }
            }
        }
   }

   stage('Deploy') {
        stages {
 
            stage('Deploy PROD') {
                when {
                    branch 'master'
                    expression {
                        currentBuild.result == null || currentBuild.result == 'SUCCESS' 
                      }
                }
                steps {
                    echo 'Deploying PROD...'
                    fileOperations([folderCopyOperation(
                        sourceFolderPath: "/bin/Release/netcoreapp3.1/publish", 
                        destinationFolderPath: "${env.PUBLISH_TARGET_PATH_PROD}"
                        )])
                    echo "Deployed on ${env.PUBLISH_TARGET_PATH_PROD}"
                    
                  
                }
            }
        }
   }

  }
  
  post {
         success {  
             echo 'This will run only if successful'  
         }  
         failure {  
             echo 'Failure'  
             mail to: '${env.EMAIL_FAILURE}',
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Project: ${env.JOB_NAME} <br/>Build Number: ${env.BUILD_NUMBER} <br/> URL de build: ${env.BUILD_URL}",
             mimeType: 'text/html'
         }  
         unstable {  
             echo 'This will run only if the run was marked as unstable'  
             mail to: '${env.EMAIL_FAILURE}',
             subject: "Unstable Pipeline: ${currentBuild.fullDisplayName}",
             body: "Project: ${env.JOB_NAME} <br/>Build Number: ${env.BUILD_NUMBER} <br/> URL de build: ${env.BUILD_URL}",
             mimeType: 'text/html'
         }  
         changed {  
             echo 'This will run only if the state of the Pipeline has changed'  
             echo 'For example, if the Pipeline was previously failing but is now successful'  
         }  
    }
 }
