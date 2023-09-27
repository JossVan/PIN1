pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    NEXUS_CREDENTIALS = credentials('nexus_credentials')
  }
   stages {
   stage('Building image') {
      steps{
          sh '''
          docker login 127.0.0.1:8082 -u ${NEXUS_CREDENTIALS.username} -p ${NEXUS_CREDENTIALS.password} 
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
        docker tag testapp 127.0.0.1:8082/repository/docker-hosted/testapp:v1.0
        docker push 127.0.0.1:8082/repository/docker-hosted/testapp:v1.0
        '''
        }
      }
    }
}


    
  

