pipeline {
  agent any
  tools {
    maven 'maven'
  }
  stages{
    stage('1-cloning project repo'){
      steps{
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/mpndevops10/jenkins-01.git']])
      }
    }
    stage('2-cleanws'){
      steps{
        sh 'mvn clean'
      }
    }
    stage('3-mavenbuild'){
      steps{
        sh 'mvn package'
      }
    }
    stage('4-codequality'){
        steps{
       sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=team10 \
  -Dsonar.host.url=http://18.220.33.167:9000 \
  -Dsonar.login=sqp_2768810549e4e13a8da045f22af0a44837852f3f"
      }
    }
    stage('5-deploy-to-tomcat') {
    steps {
        withEnv(['WAR_FILE_PATH=~/workspace/maven-build/MavenEnterpriseApp-web/target/MavenEnterpriseApplication.war']) {
            sshagent(['tomcat']) {
                sh """
                scp -o StrictHostKeyChecking=no ${WAR_FILE_PATH} ubuntu@3.21.102.135:/opt/tomcat/apache-tomcat-9.0.93/webapps
                """
            }
        }
    }
}


  }
}
