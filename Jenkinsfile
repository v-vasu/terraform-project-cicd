pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
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
         
        // stage3 : Publish the artifacts to nexus server
           stage ('Publish to nexus'){
	   steps { 
nexusArtifactUploader artifacts:[artifactId:'VinayDevOpsLab', classifier:'', file:'/target/VinayDevOpsLab-0.0.4.war', type:'war']], credentialsId:'93730ba9-6058-4eb4-af32-e5c2434ae15a', groupId: 'com.vinaysdevopslab', nexusUrl:'172.20.10.205:8081', nexusVersion:'nexus3', protocol:'http', repository:'VinaysDevOpsLab-SNAPSHOT', version:'0.0.4'

}
}
	 
}

}
