pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'C:\\Learn\\apache-tomcat-8.5.34\\tomcat-production\\webapps', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'C:\\Learn\\apache-tomcat-8.5.34\\tomcat-staging\\webapps', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                        bat "copy **\\target\\*.war ${params.tomcat_dev}"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "copy **\\target\\*.war {params.tomcat_prod}"
                    }
                }
            }
        }
    }
}