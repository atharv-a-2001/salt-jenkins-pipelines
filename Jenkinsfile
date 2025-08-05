pipeline {
    agent any

    stages {
        stage('Clone SLS Repo') {
            steps {
                git 'https://github.com/atharv-a-2001/saltstack-files.git'
            }
        }

        stage('Copy SLS to Salt Master') {
            steps {
                sh 'cp nginx-jenkins.sls /srv/salt/'
                sh 'cp nginx-start-jenkins.sls /srv/salt/'
            }
        }

        stage('Install Nginx') {
            steps {
                sh "salt '*' state.apply nginx-jenkins"
            }
        }

        stage('Start Nginx') {
            steps {
                sh "salt '*' state.apply nginx-start-jenkins"
            }
        }
    }
}
