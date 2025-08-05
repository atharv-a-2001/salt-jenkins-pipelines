pipeline {
  agent any

  stages {
    stage('Clone & Copy SLS to Salt Master') {
      steps {
        sh '''
          cp nginx-jenkins.sls /srv/salt/
          cp nginx-start-jenkins.sls /srv/salt/
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
          servername: 'http://your-salt-api-server:8000'
        )

        script {
          def output = readFile "${env.WORKSPACE}/saltOutput.json"
          echo output
        }
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
          servername: 'http://your-salt-api-server:8000'
        )

        script {
          def output = readFile "${env.WORKSPACE}/saltOutput.json"
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
