pipeline {
    agent any
    options {
        skipDefaultCheckout()
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
    }
    stages {
        stage('Checkout') {
            steps {
                slackSend(color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
                checkout scm
            }
        }
        stage('Deploy develop') {
            when {
                branch 'develop'
            }
            steps {
                sh 'eval rsync $RSYNC_PARAMS        ./    $DESTINATION_SERVER_DEVELOP:$DESTINATION_PATH_DEVELOP/'
            }
        }
    }
    post {
        always{
            cleanWs()
        }
        success {
            slackSend(color: '#66CDAA', message: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure {
            slackSend(color: '#FF4040', message: "FAIL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }
    environment {
        DESTINATION_SERVER_DEVELOP = 'www-data@104.199.5.93'
        DESTINATION_PATH_DEVELOP = '/var/www/html'
        DESTINATION_CONFIGCOMPOSER = './'
        RSYNC_PARAMS = '-rlvv --del --exclude=.git --exclude-from=.rsync_exclude --exclude .rsync_exclude --exclude rsync.log --log-file=rsync.log '
    }
}
