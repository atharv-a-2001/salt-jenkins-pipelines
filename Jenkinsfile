pipeline {
    agent any
    environment {
        SALT_API_URL = 'http://34.148.114.59:8000'
    }

    stages {
        stage('Update GitFS Cache') {
            steps {
                salt authtype: 'basic',
                     clientInterface: 'runner',
                     credentialsId: 'jenkins',   // Jenkins credentials ID
                     function: 'saltutil.sync_all',
                     servername: "${env.SALT_API_URL}"
            }
        }

        stage('Install Nginx') {
            steps {
                salt authtype: 'basic',
                     clientInterface: 'local',
                     credentialsId: 'jenkins',
                     function: 'state.apply',
                     target: '*',
                     fileName: 'nginx-jenkins',   // Your .sls file in repo
                     servername: "${env.SALT_API_URL}"
            }
        }

        stage('Start nginx') {
            steps {
                salt authtype: 'basic',
                     clientInterface: 'local',
                     credentialsId: 'jenkins',
                     function: 'service.start',
                     target: '*',
                     arguments: 'nginx',
                     servername: "${env.SALT_API_URL}"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
