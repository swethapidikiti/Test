pipeline{
    //Directives
    agent any
    tools{
        maven 'maven'
    }
    environment{
        artifactId = readMavenPom().getArtifactId()
        version = readMavenPom().getVersion()
        name = readMavenPom().getName()
        groupId = readMavenPom().getGroupId()
    }


    stages{
        //Stage 1
        stage('Build'){
            steps{
                sh 'mvn clean install package'
            }
        }
        //Stage 2
        stage('Test'){
            steps{
                echo 'testing....'
            }
        }
        //Stage 3: Publish artifacts to Nexus
        stage('Publish to Nexus'){
            steps{
                script{
                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "SwethaDevOpsLab-SNAPSHOT" : "SwethaDevOpsLab-RELEASE"
      
                    nexusArtifactUploader artifacts:
                     [[artifactId: "${artifactId}", classifier: '', 
                    file: 'target/SwethaLab-0.0.4-SNAPSHOT.war', type: 'war']], 
                    credentialsId: 'd71323f2-a0a5-4ae3-955b-044b06527a9b',
                    groupId: "${groupId}", nexusUrl: '172.20.10.211:8081',
                    nexusVersion: 'nexus3', protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${version}"
                }
            }
        }
        //Stage 4 : Print some information
        stage('Print Environment variables')
        {   
            steps{
                echo "Artifact ID is '${artifactId}'"
                echo "Version is '${version}'"
                echo "Name is '${name}'"
            }
        }
        // Stage 5
        stage('Deploy'){
            steps{
                echo 'Deploying...'
                sshPublisher(publishers: 
                [sshPublisherDesc(configName: 'Ansible_Controller',
                transfers: [
                    sshTransfer(
                        cleanRemote: false, 
                        excludes: '', 
                        execCommand: 'ansible-playbook /opt/playbooks/downloadandDeploy.yml -i /opt/playbooks/hosts', 
                        execTimeout: 120000, 
                        )],
                usePromotionTimestamp: false,
                useWorkspaceInPromotion: false,
                verbose: false)])
            }
        }
    }
}