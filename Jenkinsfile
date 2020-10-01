pipeline {
    agent any
    tools {
  maven 'maven3'
}

    stages {
       
        stage('maven build') {
            steps {
                sh 'mvn clean package'
            }
        }
stage('nexus upload') {
            steps {
                script{
                def pomFile = readMavenPom file: 'pom.xml'
                def version = pomFile.version
                def nexusRepo = version.endsWith("SNAPSHOT") ? "my-app-snapshot" : "my-app-release"
                nexusArtifactUploader artifacts: [[artifactId: 'my-app', classifier: '', file: 'target/my-app.war', type: 'war']], 
                    credentialsId: 'Nexus', 
                    groupId: 'in.javahome', 
                    nexusUrl: '13.233.245.222:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepo, 
                    version: version
                }
            }

        }
          stage('Deploy tomcat') 
          {
            steps {
                sshagent(['tomcat-dev']) 
                {
                sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@3.6.41.96:/opt/tomcat8/webapp/my-app.war"
                sh "ec2-user@3.6.41.96 /opt/tomcat8/bin/shutdown.sh"
                sh "ec2-user@3.6.41.96 /opt/tomcat8/bin/startup.sh"
                
                 }
              }
        }
    }
}
