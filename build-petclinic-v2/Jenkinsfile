pipeline {
    agent {
        label "master"
    }
    parameters {
        string defaultValue: '2.7.3-SNAPSHOT', name: 'VERSION'
    }
    tools {
        maven 'maven'
    }
     environment {
        NEXUS_URL = "https://nexus-lab.pp.ua"
        NEXUS_REPOSITORY = "maven-lab"
        NEXUS_CREDENTIAL_ID = "nexus-user-credentials"
        MAVEN_REPO = "maven-lab"
        GROUP_ID = "org.springframework.samples"
        ARTIFACT_ID = "spring-petclinic"
        FILE_EXTENSION = "jar"
        
    }
   stages {
        stage('Download artifact from Nexus Repository Manager') {
           steps {
              withCredentials([usernamePassword(credentialsId: 'nexus-user-credentials', passwordVariable: 'PASSWD', usernameVariable: 'USER')]) {
                 sh '''#!/bin/bash
                     download_url=$(curl --user $USER:$PASSWD -X GET "${NEXUS_URL}/service/rest/v1/search/assets?repository=${MAVEN_REPO}&maven.groupId=${GROUP_ID}&maven.artifactId=${ARTIFACT_ID}&maven.baseVersion=${VERSION}&maven.extension=${FILE_EXTENSION}" -H  "accept: application/json"  | jq -rc '.items | .[].downloadUrl' | sort | tail -n 1)
                     wget --user=\$USER --password=\$PASSWD $download_url
                    '''
              }
              script {
                   sh 'java -jar ./*.jar'
              }
           }
        }    
   } 
}
