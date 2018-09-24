
pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '40.122.37.24', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '40.117.82.78', description: 'Production Server')
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

        stage ('Deployment'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/talasila/mykeys/tomcat **/target/*.war talasila@${params.tomcat_dev}:/opt/tomcat/webapps"
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/talasila/mykeys/private **/target/*.war talasila@${params.tomcat_prod}:/home/talasila/mykeys/tomcat"
                    }
                }

            }
        }
    }
}
