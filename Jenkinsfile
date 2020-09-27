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
    }
}
