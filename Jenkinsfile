pipeline {
    //Directives
    agent any
    tools {
        maven 'Maven'
        }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        //stage 3 : Publish the artifacts to Nexus
        stage ('publish to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'MohanDevOpsLab', classifier: '', file: 'target/MohanDevOpsLab-0.0.2_SNAPSHOT.war', type: 'war']], credentialsId: '6581f915-5dce-4c8c-bbc5-876f93bf50a1', groupId: 'com.mohansdevopslab', nexusUrl: '13.235.248.58:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'MohanDevOpsLab-SNAPSHOT', version: '0.0.2_SNAPSHOT'
            }

        }
        //stage 4 : Deploy
    stage ('deploy') {
            steps {
                echo 'deploying...!'
            }
        }
        // Stage3 : Publish the source code to Sonarqube
        /*stage ('Sonarqube Analysis'){
            steps {
                echo ' Source code published to Sonarqube for SCA......'
                withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                     sh 'mvn sonar:sonar'
                }

            }*/
    }

        
        
}
