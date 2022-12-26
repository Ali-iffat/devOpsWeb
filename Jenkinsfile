pipeline {
    agent any
    

    tools {
        maven '3.8.6'
    }
    
    parameters {
         string(name: 'staging_server', defaultValue: '3.92.176.142', description: 'Remote Staging Server')
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
                        sh "scp -vvv -o StrictHostKeyChecking=no **/*.war ubuntu@${params.staging_server}:/opt/tomcat/webapps/"
                    }
                }
            }
        }
    }
}
