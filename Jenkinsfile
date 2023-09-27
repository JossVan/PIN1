pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }
   stages {
   stage('Building image') {
      steps{
          script {
                def nexusCredentials = credentials('nexus_credentials')
                def dockerUsername = nexusCredentials.username
                def dockerPassword = nexusCredentials.password
                sh """
                docker login 127.0.0.1:8082 -u ${dockerUsername} -p ${dockerPassword}
                docker build -t testapp .
                """

            }
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


    
  

