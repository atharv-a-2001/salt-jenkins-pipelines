pipeline {
    agent any

    environment {
        SALT_API = 'http://34.148.114.59:8000'  // Replace with your Salt API endpoint
    }

    stages {
        stage('Update GitFS Cache') {
            steps {
                script {
                    salt authtype: 'pam',
                         credentialsId: 'git-jenkins-salt',
                         servername: "${env.SALT_API}",
                         clientInterface: runner(
                             function: 'fileserver.update',
                             arguments: '' // MUST be empty string, NOT []
                         )
                }
            }
        }

        stage('Install Nginx') {
            steps {
                script {
                    salt authtype: 'pam',
                         credentialsId: 'git-jenkins-salt',
                         servername: "${env.SALT_API}",
                         clientInterface: local(
                             tgt: '*',
                             function: 'state.apply',
                             arguments: 'nginx.install'
                         )
                }
            }
        }

        stage('Start Nginx') {
            steps {
                script {
                    salt authtype: 'pam',
                         credentialsId: 'git-jenkins-salt',
                         servername: "${env.SALT_API}",
                         clientInterface: local(
                             tgt: '*',
                             function: 'service.start',
                             arguments: 'nginx'
                         )
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
