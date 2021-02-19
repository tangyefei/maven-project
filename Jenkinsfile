pipeline {
    agent any

    tools{
        maven 'local maven'
    }

    parameters{
        string(name: 'tomcat_dev', defaultValue: '47.98.33.167', description: 'Staging Server')
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
                    echo '开始存档...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

     stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /Users/eleme/Documents/toys/aliyun-staging-secret-token.pem **/target/*.war root-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}