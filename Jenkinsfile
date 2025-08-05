pipeline {
  agent any

  environment {
    SALT_API = 'http://34.148.114.59:8000'
    SALT_CREDENTIALS = 'git-jenkins-salt'
    SALT_MINION = 'windowsminion'
    SLS_FILE = 'nginx-jenkins'
  }

  stages {
    stage('Update GitFS Cache') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: runner(function: 'fileserver.update'),
          credentialsId: SALT_CREDENTIALS,
          servername: SALT_API
        )
      }
    }

    stage('Install Nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            arguments: [SLS_FILE],
            function: 'state.apply',
            target: SALT_MINION
          ),
          credentialsId: SALT_CREDENTIALS,
          servername: SALT_API
        )
      }
    }

    stage('Start nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            arguments: ['nginx'],
            function: 'service.start',
            target: SALT_MINION
          ),
          credentialsId: SALT_CREDENTIALS,
          servername: SALT_API
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
