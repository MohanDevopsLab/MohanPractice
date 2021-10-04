pipeline {  
    //Directives
    agent any
    tools {
        maven 'Maven'
        }
    environment {
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name =  readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()

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
                script {
                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "MohanDevOpsLab_SNAPSHOT" : "MohanDevOpsLab_RELEASE"

                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: '0c658c3a-7a29-4a38-9c5a-e435738cac03', 
                groupId: "${GroupId}", 
                nexusUrl: '35.154.42.153:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http',
                repository: "${NexusRepo}", 
                version: "${Version}"
                }
            }

        }
        //Stage 4: Print some Information
        stage ('publish Environmental Variables'){
            steps {
                echo "ArtifactId is '${ArtifactId}'"
                echo "GroupId is '${GroupId}'"
                echo "Version is '${Version}'"
                echo "Name is '${Name}'"
            }
        }

        //stage 5 : Deploy to tomcat

        stage ('deploy') {
            steps {
                echo 'deploying...!'
                sshPublisher(publishers: [sshPublisherDesc(
                    configName: 'AnsibleController', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false, 
                            excludes: '', 
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy-tomcat.yaml -i /opt/playbooks/hosts', 
                            execTimeout: 120000, 
                            flatten: false, 
                            makeEmptyDirs: false, 
                            noDefaultExcludes: false, 
                            patternSeparator: '[, ]+', 
                            remoteDirectory: '', 
                            remoteDirectorySDF: false, 
                            removePrefix: '', 
                            sourceFiles: '')], 
                    usePromotionTimestamp: false,
                    useWorkspaceInPromotion: false,
                    verbose: false)])

            
            }
        }

        //stage 6 : Deploy to Docker
        stage ('deploy to Docker') {
            steps {
                echo 'deploying...!'
                sshPublisher(publishers: [sshPublisherDesc(
                    configName: 'AnsibleController', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false, 
                            excludes: '', 
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts', 
                            execTimeout: 120000, 
                            flatten: false, 
                            makeEmptyDirs: false, 
                            noDefaultExcludes: false, 
                            patternSeparator: '[, ]+', 
                            remoteDirectory: '', 
                            remoteDirectorySDF: false, 
                            removePrefix: '', 
                            sourceFiles: '')], 
                    usePromotionTimestamp: false,
                    useWorkspaceInPromotion: false,
                    verbose: false)])

            
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
