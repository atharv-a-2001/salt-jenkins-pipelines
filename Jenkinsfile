pipeline {
  agent any

  stages {
    stage('Copy SLS to Salt Master') {
      steps {
        sh '''
          cp ${WORKSPACE}/nginx-jenkins.sls /srv/salt/
          cp ${WORKSPACE}/nginx-start-jenkins.sls /srv/salt/
        '''
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
            jobPollTime: 6,
            target: '*',
            targettype: 'glob'
          ),
          credentialsId: 'jenkins',
          saveFile: true,
          servername: 'http://34.148.114.59:8000'
        )
      }
    }

    stage('Start Nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            function: 'state.apply',
            arguments: 'nginx-start-jenkins',
            blockbuild: true,
            jobPollTime: 6,
            target: '*',
            targettype: 'glob'
          ),
          credentialsId: 'jenkins',
          saveFile: true,
          servername: 'http://34.148.114.59:8000'
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
