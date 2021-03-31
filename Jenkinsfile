pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
       ArtifactId = readMavenPom().getArtifactId()
       Version = readMavenPom().getVersion()
       Name = readMavenPom().getName()
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
                echo 'testing......'

            }
        }
        // Stage3 : Publish the artifacts to nexus
        stage ('Publish to Nexus'){
           steps {
               script {
               def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"
               nexusArtifactUploader artifacts: 
               [[artifactId: "${ArtifactId}", 
               classifier: '', 
               file: "target/${ArtifactId}-${Version}.war", 
               type: 'war']], 
               credentialsId: '93730ba9-6058-4eb4-af32-e5c2434ae15a', 
               groupId: "${GroupId}", 
               nexusUrl: '172.20.10.205:8081', 
               nexusVersion: 'nexus3', 
               protocol: 'http', 
               repository: "${NexusRepo}",
               version: "${Version}"
           }
        }
        }

        // Stage 4 : Print some information
        stage ('Print Environment variables'){
                    steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "GroupID is '${GroupId}'"
                        echo "Name is '${Name}'"
                    }
                }
	    // Stage5 : Deploying
	    stage ('Deploy') {
	        steps { 
	            echo 'Deploying......'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible_Controller', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yml -i /opt/playbooks/hosts', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

        }
	    }
        // Stage6 : Deploying the build artifact to Docker 
	    /*stage ('Deploy to Docker ') {
	        /* { 
	            echo 'Deploying......'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible_Controller', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /opt/playboogitks/downloadanddeploy_docker.yml -i /opt/playbooks/hosts', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

            }
        }*/



    }
}
