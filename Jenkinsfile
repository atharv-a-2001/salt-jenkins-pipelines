pipeline {
    agent any

    environment {
        SALT_MASTER_IP = "34.148.114.59"
        SALT_MINION_ID = "saltmaster.us-east1-d.c.piby3-finops.internal"
    }

    stages {
        stage('Clone SLS Files') {
            steps {
                echo "Cloning SLS repo from GitHub..."
                sh 'git clone https://github.com/atharv-a-2001/saltstack-files.git sls-files'
            }
        }

        stage('Copy to Salt Master') {
            steps {
                echo "Copying SLS files to /srv/salt..."
                sh 'sudo cp sls-files/*.sls /srv/salt/'
            }
        }

        stage('Install Nginx') {
            steps {
                echo "Applying nginx-jenkins state on minion..."
                sh 'sudo salt $SALT_MINION_ID state.apply nginx-jenkins'
            }
        }

        stage('Start Nginx') {
            steps {
                echo "Applying nginx-start-jenkins state on minion..."
                sh 'sudo salt $SALT_MINION_ID state.apply nginx-start-jenkins'
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs()
        }
    }
}
