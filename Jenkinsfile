pipeline {
  agent { label 'jenkins-node' }
  
  parameters {
    string defaultValue: '172.31.45.161', name: 'TOMCAT_SERVER'
    string defaultValue: '/var/lib/tomcat9/webapps', name: 'TOMCAT_ROOT'
    string defaultValue: 'ubuntu', name: 'TOMCAT_USER'
  }
  
  triggers {
    pollSCM '* * * * *'
  }

  
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/kogo28/source-maven-java-spring-hello-webapp.git'
      }
    }

    stage('Maven Build') {
      tools { maven 'Maven-3' }
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Deploy to Tomcat') {
      steps {
        sh "scp {env.WORKSPACE}/target/hello-world.war ${params.TOMCAT_USER}@${params.TOMCAT_SERVER}:${params.TOMCAT_ROOT}"
      }
    }
  }
}
