pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '3.18.108.174', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.223.152.28', description: 'Production Server')
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
					    echo Entered Staging
                        sh "echo y | scp -i /home/indu/.ssh/tomcat-ubuntu.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
					     echo Entered Production
                        sh "echo y | scp -i /home/indu/.ssh/tomcat-ubuntu.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}