pipeline {
    agent any

    tools{
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.90.87.6', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.90.188.107', description: 'Production Server')
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
                        
                        sh "scp -i /Users/mustafaalogaidi/Desktop/SSH/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        
                        sh "scp -i /Users/mustafaalogaidi/Desktop/SSH/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}