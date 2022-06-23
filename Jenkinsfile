pipeline {
    agent any
    
    
    stages {
        stage('Run') {
             when {
                branch 'master'
            }
            steps {
                echo 'Running build automation'
                sh 'mvn clean install --settings configuration/settings.xml'
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
       
        
        stage('build') {
             when {
                branch 'master'
            }
            steps {
                echo 'Running build automation'
                sh 'mvn --settings configuration/settings.xml fabric8:build -Pkubernetes-deployment -DskipTests -Dfabric8.generator.spring-boot.name=balance:inquiry${env.BUILD_NUMBER}'
                
            }
        }
        
        stage('Tag and Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        
        stage('deploy') {
             when {
                branch 'master'
            }
            steps {
                echo 'Running deploy automation'
                sh 'mvn fabric8:deploy -kubernetes'
                
                
            }
        }
        
         
       
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }

        


        stage('Deploy') {
             when {
                branch 'master'
            }
            steps {
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
                )
            }
        }


        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    
        
    }
}
