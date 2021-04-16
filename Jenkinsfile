pipeline {
    agent {
        label 'master'
        }
        tools {
           maven 'mvn-3.6.3'    
        }
        triggers {
            pollSCM('*/5 * * * *')
        }
        options {
         buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '2')
         timestamps()
        }
        stages {
            stage('Git Clone') {
                steps {
                    git branch 'Dev' credentialsId: 'abc14a2a-3c68-43e0-a31c-7776604d8aad', url: 'https://github.com/vikash463/Multi-branch.git'
                    }
                }
                stage('Build') {
                    steps {
                        sh 'mvn clean package'
                    }
                }
                stage('Sonar') {
                    steps {
                    sh 'mvn sonar:sonar'
                    }
                }
                stage('Artifacts') {
                    steps {
                        sh 'mvn clean deploy'
                    }
                }
                stage('Tomcat Deployment') {
                    steps {
                      sshagent(['31017e99-d7b5-41b6-a3ee-99138db8074d']) {
                        sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.19.152:/opt/tomcat/webapps/'
                    }
                }
            }
        }
     post { 
         success {
             emailext body: '''Build is completed..

             Regards,
             Vikash TS''', subject: 'Web project Build - Declarative', to: 'vikashtimiri@gmail.com'
         }
         failure {
             emailext body: '''Build is failure..

             Regards,
             Vikash TS''', subject: 'Web project Build - Failure - Declarative', to: 'vikashtimiri@gmail.com'
         }
     }
  }
