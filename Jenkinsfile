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
                echo ' testing......'

            }
        }

        //

        // stage ('Publish to Nexus'){
        //     steps{
        //         nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: '6ca91e13-aaa7-4655-a02d-3a68119826fb', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.108:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'VinaysDevOpsLab-SNAPSHOT', version: '0.0.4-SNAPSHOT'
        //     }
        // }

                // Stage3 : Publish the artifacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                script {

                def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"

                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: '35e9b26e-269a-4804-a70d-6b2ec7a608ce', 
                groupId: "${GroupId}", 
                nexusUrl: '54.219.207.218:8081', 
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

        // Stage3 : Publish the source code to Sonarqube
        stage ('Sonarqube Analysis'){
            steps {
                echo ' Source code published to Sonarqube for SCA......'
                // withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                //      sh 'mvn sonar:sonar'
                // }

            }
        }

        
        
    }

}