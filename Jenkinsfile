pipeline {
    agent { label 'main' }
    

    tools {
        maven '3.6.3'
    }
    // ... stages ...///
}
    parameters {
         string(name: 'staging_server', defaultValue: '44.202.65.218', description: 'Remote Staging Server')
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
                        sh "scp -v -o StrictHostKeyChecking=no **/*.war root@${params.staging_server}:/opt/tomcat/webapps/"
                    }
                }
            }
        }
    }
}
