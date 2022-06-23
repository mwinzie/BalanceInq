pipeline {
    agent any 
    stages {      
        stage('Build') { 
            steps {
                echo 'Running build automation'
                sh 'mvn clean install --settings configuration/settings.xml'
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
        stage('Test') { 

        }
        stage('Deploy') { 

        }
    }
}
