pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue:'10.0.10.84', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '10.0.10.188', description: 'Production Server')
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
                        sh "scp -i /home/jenkins/ng_van.chinh.pem webapp/target/*.war ubuntu@${params.tomcat_dev}:/opt/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /homw/jenkins/ng_van.chinh.pem webapp/target/*.war ubuntu@${params.tomcat_prod}:/opt/tomcat/webapps"
                    }
                }
            }
        }
    }
}
