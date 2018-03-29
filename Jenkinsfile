pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.206.75.115', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.147.124.185', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/ubuntu/ng_van.chinh.pem **/target/*.war ubuntu@${params.tomcat_dev}:/opt/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/ubuntu/ng_van.chinh.pem **/target/*.war ubuntu@${params.tomcat_prod}:/opt/tomcat/webapps"
                    }
                }
            }
        }
    }
}
