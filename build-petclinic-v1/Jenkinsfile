pipeline {
   agent any
  
   tools {
     maven 'maven'
   }
  
   stages {
     stage("Clone code from git") {
            steps {
                script {
                    git branch: 'main', 
                       url: 'https://github.com/spring-projects/spring-petclinic.git'
                    
                }
            }
     }
     
     stage("Build") {
              steps {
                      withMaven(maven: 'maven', mavenSettingsConfig: 'mvn-setting-xml') {
                      sh "mvn clean package -DskipTests=true -Dcheckstyle.skip"
                      }
               }
     }
     stage("Zip arifact and push to Nexus") {
              steps {
                      sh 'tar czvf artifact_${BUILD_NUMBER}.tar.gz target/*.jar'
                      withCredentials([usernamePassword(credentialsId: 'nexus-user-credentials', passwordVariable: 'PASSWD', usernameVariable: 'USER')]) {
                      sh "curl -v -u $USER:$PASSWD --upload-file artifact_${BUILD_NUMBER}.tar.gz ${env.NEXUS_REPO_URL}/artifact_${BUILD_NUMBER}.tar.gz"
                      }
                      sh ' echo $BUILD_NUMBER > ./var_build'
                
              }    
      }
   }
}
