pipeline {
  agent any

  environment {
    SALT_API = 'http://34.148.114.59:8000'
  }

  stages {
    stage('Update GitFS Cache') {
      steps {
        script {
          salt(
            authtype: 'pam',
            credentialsId: 'git-jenkins-salt',
            servername: "${env.SALT_API}",
            clientInterface: runner(
              function: 'fileserver.update'
            )
          )
        }
      }
    }

    stage('Install Nginx') {
      steps {
        script {
          salt(
            authtype: 'pam',
            credentialsId: 'git-jenkins-salt',
            servername: "${env.SALT_API}",
            clientInterface: local(
              function: 'state.apply',
              arguments: 'nginx_jenkins',
              blockbuild: true,
              jobPollTime: 6,
              target: '*',
              targettype: 'glob'
            ),
            saveFile: true
          )

          def output = readFile("${env.WORKSPACE}/saltOutput.json")
          echo output
        }
      }
    }

    stage('Start Nginx') {
      steps {
        script {
          salt(
            authtype: 'pam',
            credentialsId: 'git-jenkins-salt',
            servername: "${env.SALT_API}",
            clientInterface: local(
              function: 'state.apply',
              arguments: 'nginx_start_jenkins',
              blockbuild: true,
              jobPollTime: 6,
              target: '*',
              targettype: 'glob'
            ),
            saveFile: true
          )

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
