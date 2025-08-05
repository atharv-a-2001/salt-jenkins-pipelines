pipeline {
  agent any

  stages {
    stage('Update GitFS Cache') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: runner(
            function: 'fileserver.update'
          ),
          credentialsId: 'git-jenkins-salt',
          servername: 'http://34.148.114.59:8000'
        )
      }
    }

    stage('Install Nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            function: 'state.apply',
            arguments: 'nginx_jenkins',
            blockbuild: true,
            jobPollTime: 6,
            target: '*',
            targettype: 'glob'
          ),
          credentialsId: 'git-jenkins-salt',
          saveFile: true,
          servername: 'http://34.148.114.59:8000'
        )

        script {
          def output = readFile "${env.WORKSPACE}/saltOutput.json"
          echo output
        }
      }
    }

    stage('Start nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            function: 'state.apply',
            arguments: 'nginx_start_jenkins',
            blockbuild: true,
            jobPollTime: 6,
            target: '*',
            targettype: 'glob'
          ),
          credentialsId: 'git-jenkins-salt',
          saveFile: true,
          servername: 'http://34.148.114.59:8000'
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

