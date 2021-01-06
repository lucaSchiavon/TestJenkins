pipeline {
 agent any
 
 environment {
   
    PROJECT_NAME = "AppAsp2"
    PUBLISH_TARGET_PATH_PROD = "D:\\WWWROOT\\TestJenkins"

}

 stages {

  stage('Build') {
    stages {

        stage('bild PROD') {
            when {
                branch 'master'
            }
            steps {
                echo 'Building PROD...xx2'
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
                        sourceFolderPath: "${workspace}/bin/Release/netcoreapp3.1/publish", 
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
            
         }  
         unstable {  
             echo 'This will run only if the run was marked as unstable'  
           
         }  
         changed {  
             echo 'This will run only if the state of the Pipeline has changed'  
             echo 'For example, if the Pipeline was previously failing but is now successful'  
         }  
    }
 }
