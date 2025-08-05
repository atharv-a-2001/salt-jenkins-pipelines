pipeline {
  agent any

  environment {
    SLS_REPO = 'https://github.com/atharv-a-2001/saltstack-files.git'
    SALT_MASTER = 'http://34.148.114.59:8000'
  }

  stages {
    stage('Clone SLS Files') {
      steps {
        sh 'git clone ${SLS_REPO} sls-files'
      }
    }

    stage('Copy to Salt Master') {
      steps {
        sh 'cp sls-files/*.sls /srv/salt/'
      }
    }

    stage('Install Nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            function: 'state.apply',
            arguments: 'nginx-jenkins',
            blockbuild: true,
            target: '*',
            targettype: 'glob'
          ),
          credentialsId: 'jenkins',
          saveFile: true,
          servername: "${SALT_MASTER}"
        )

        script {
          def output = readFile("${env.WORKSPACE}/saltOutput.json")
          echo output
        }
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}
