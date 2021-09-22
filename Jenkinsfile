pipeline{
    //Directives
    agent any
    tools{
        maven 'maven'
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
                nexusArtifactUploader artifacts: [[artifactId: 'SwethaLab', classifier: '', 
                file: 'target/SwethaLab-0.0.3-SNAPSHOT.war', type: 'war']], 
                credentialsId: 'd71323f2-a0a5-4ae3-955b-044b06527a9b',
                groupId: 'com.Swethalab', nexusUrl: '172.20.10.46:8081',
                nexusVersion: 'nexus3', protocol: 'http', 
                repository: 'SwethaDevOpsLab-SNAPSHOT', 
                version: '0.0.4-SNAPSHOT'
            }
        }
        // Stage 4
        stage('Deploy'){
            steps{
                echo 'Deploying...'
            }
        }
    }
}