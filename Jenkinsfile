pipeline {
    agent any
    environment {
        SALT_API_URL = 'http://34.148.114.59:8000'
    }

    stages {
        stage('Update GitFS Cache') {
            steps {
                saltRunner(
                    credentialsId: 'git-jenkins-salt',
                    servername: "${env.SALT_API_URL}",
                    function: 'saltutil.sync_all'
                )
            }
        }

        stage('Install Nginx') {
            steps {
                saltCommand(
                    credentialsId: 'git-jenkins-salt',
                    servername: "${env.SALT_API_URL}",
                    function: 'state.apply',
                    target: '*',
                    arguments: 'nginx-jenkins'
                )
            }
        }

        stage('Start nginx') {
            steps {
                saltCommand(
                    credentialsId: 'git-jenkins-salt',
                    servername: "${env.SALT_API_URL}",
                    function: 'service.start',
                    target: '*',
                    arguments: 'nginx'
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
