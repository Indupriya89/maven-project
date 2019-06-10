pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.220.168.252', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.216.118.225', description: 'Production Server')
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
					    echo 'Enter Staging'
                        sh "cp -i **/target/*.war /opt/tomcat/apache-tomcat-9.0.19/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
					     echo 'Entered Production'
                        sh "cp -i **/target/*.war /opt/tomcat/apache-tomcat-prod/webapps"
                    }
                }
            }
        }
    }
}