pipeline {
  agent any

  environment {
    SALT_API = 'http://34.148.114.59:8000'
    SALT_CREDENTIALS = 'git-jenkins-salt'
    SALT_MINION = 'windowsminion'
    SLS_FILE = 'nginx-jenkins'
    OUTPUT_FILE = "${env.WORKSPACE}/saltOutput.json"
  }

  stages {
    stage('Update GitFS Cache') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: runner(function: 'fileserver.update'),
          credentialsId: "${env.SALT_CREDENTIALS}",
          servername: "${env.SALT_API}"
        )
      }
    }

    stage('Install Nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            arguments: [env.SLS_FILE],
            function: 'state.apply',
            target: env.SALT_MINION
          ),
          credentialsId: "${env.SALT_CREDENTIALS}",
          servername: "${env.SALT_API}",
          saveFile: true,
          fileName: env.OUTPUT_FILE
        )
      }
    }

    stage('Check Salt Output') {
      steps {
        script {
          if (fileExists(env.OUTPUT_FILE)) {
            def output = readFile(env.OUTPUT_FILE)
            echo output
          } else {
            echo "Salt output file not found"
          }
        }
      }
    }

    stage('Start nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            arguments: ['nginx'],
            function: 'service.start',
            target: env.SALT_MINION
          ),
          credentialsId: "${env.SALT_CREDENTIALS}",
          servername: "${env.SALT_API}"
        )
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}
