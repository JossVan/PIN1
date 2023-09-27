pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }
   stages {
   stage('Building image') {
      environment {
        NEXUS_CREDENTIALS = credentials('nexus_credentials')
      }
      steps {
              sh '''
                echo "$NEXUS_CREDENTIALS_PSW" | docker login -u "$NEXUS_CREDENTIALS_USR" --password-stdin 127.0.0.1:8083
                docker build -t testapp .
                '''
        }
    }  
    stage('Run tests') {
      steps {
        sh "docker run testapp npm test"
      }
    }
   stage('Deploy Image') {
      steps{
        sh '''
        docker tag testapp 127.0.0.1:8083/repository/docker-hosted/testapp:v1.0
        docker push 127.0.0.1:8083/repository/docker-hosted/testapp:v1.0
        '''
        }
      }
    }
}


    
  

