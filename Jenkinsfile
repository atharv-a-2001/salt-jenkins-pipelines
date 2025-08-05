pipeline {
    agent any

    environment {
        SALT_API_URL = 'http://34.148.114.59:8000'
    }

    stages {
        stage('Install Nginx via Salt') {
            steps {
                salt(
                    authtype: 'basic',
                    clientInterface: 'local',
                    credentialsId: 'git-jenkins-salt',
                    function: 'state.apply',
                    target: 'saltmaster.us-east1-d.c.piby3-finops.internal',
                    arguments: 'nginx_jenkins',
                    servername: "${env.SALT_API_URL}"
                )
            }
        }

        stage('Start Nginx via Salt') {
            steps {
                salt(
                    authtype: 'basic',
                    clientInterface: 'local',
                    credentialsId: 'git-jenkins-salt',
                    function: 'state.apply',
                    target: 'saltmaster.us-east1-d.c.piby3-finops.internal',
                    arguments: 'nginx_start_jenkins',
                    servername: "${env.SALT_API_URL}"
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

