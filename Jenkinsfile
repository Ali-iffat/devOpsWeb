pipeline {
    agent any
    

    tools {
        maven '3.8.6'
    }
    
    parameters {
         string(name: 'staging_server', defaultValue: '3.93.17.22', description: 'Remote Staging Server')
    }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        sh "scp -v -o StrictHostKeyChecking=no **/*.war ubuntu@${params.staging_server}:/opt/tomcat/webapps/"
                    }
                }
            }
        }
    }
}
