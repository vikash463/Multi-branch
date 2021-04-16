pipeline {
    agent {
        label 'master'
    }
    tools {
        maven 'mvn-3.6.3'
         }
    stages {
        stage('Git') {
            steps {
                git branch: '', credentialsId: 'abc14a2a-3c68-43e0-a31c-7776604d8aad', url: 'https://github.com/vikash463/Multi-branch.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('sonar report') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }
        stage('Artifacts') {
            steps {
                sh 'mvn clean deploy'
            }
        }
        stage('Deploy to tomcat') {
            steps {
                sshagent(['31017e99-d7b5-41b6-a3ee-99138db8074d']) {
                    sh 'scp -o StrictHostKeyChecking=no target/maven-stanalone-application-0.0.1-SNAPSHOT.jar ec2-user@172.31.19.152:/opt/tomcat/webapps/'
            }
        }
    }
  }
}
