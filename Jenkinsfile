pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }
   stages {
   stage('Building image') {
      steps{
          script {
                // Obtener las credenciales
                def nexusCredentials = credentials('nexus_credentials')

                // Verificar si las credenciales son de tipo UsernamePasswordBinding
                if (nexusCredentials instanceof UsernamePasswordBinding) {
                    // Acceder al nombre de usuario y contraseña
                    def dockerUsername = nexusCredentials.username
                    def dockerPassword = nexusCredentials.password

                    // Ejecutar los comandos Docker
                    sh """
                    docker login 127.0.0.1:8082 -u ${dockerUsername} -p ${dockerPassword}
                    docker build -t testapp .
                    """
                } else {
                    error("La credencial no es de tipo UsernamePasswordBinding")
                }
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


    
  

