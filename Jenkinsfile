pipeline {
  agent {
    label 'Any'
  }
  
  environment {
    MAVEN_HOME = tool name: 'maven3.8.2'
  }
  
  stages{
    stage('1.GetCode') {
      steps {
        git "https://github.com/class-32-practice/maven-web-application-3.git"
       
      }
    }
    
    stage('2.Build') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn package"
      }
    }
    
    stage('3.codeQualityAnalysis') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn sonar:sonar"
      }
    }
    
    stage('4.upload') {
      steps {
        sh "${env.MAVEN_HOME}/bin/mvn deploy"
      }
    }
    
    stage('5.deploy2UAT') {
      steps {
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://34.238.42.192:8176/')], contextPath: null, war: 'target/*war'
      }
    }
    
    stage('6.Approval') {
      steps {
        echo 'Apps ready for review'
        timeout(time: 5, unit: 'HOURS') {
          input message: 'Application ready for deployment. Please review and approve.'
        }
      }
    }
  }
}
