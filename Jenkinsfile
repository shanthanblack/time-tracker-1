pipeline {
  agent any 
  tools {
    maven 'maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json  https://github.com/shanthanblack/time-tracker-1.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat web/target/sonar/report-task.txt'
        }
      }
    }

stage ('Build') {
      steps {
      sh 'mvn clean package'
    }
}
    stage ('Deploy-To-Tomcat') {
            steps {
                sh 'cp web/target/*.war /opt/tomcat/webapps/webapp.war'       
           }
    }
    
    
    stage ('DAST') {
      steps {
         sh 'docker run -t owasp/zap2docker-stable zap-baseline.py -t http://172.31.10.38:8090/webapps/ || true'
      }
    }
    
  }
}
