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
               file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', 
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
        }
	    }
    }
}
