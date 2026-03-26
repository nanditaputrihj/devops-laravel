node {
    checkout scm

    stage("Build") {
        docker.image('composer:2').inside('-u root --entrypoint=""') {
            sh '''
            git config --global --add safe.directory /var/jenkins_home/workspace/laravel-dev
            cd src
            composer install
            '''
        }
    }

    stage("Testing") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

    stage("Deploy to Production") {
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
            sshagent (credentials: ['ssh-prod']) {
                sh '''
                mkdir -p ~/.ssh
                ssh-keyscan -H $PROD_HOST >> ~/.ssh/known_hosts

                echo "TEST SSH CONNECTION..."
                ssh -o StrictHostKeyChecking=no ubuntu@$PROD_HOST "echo CONNECTED"

                echo "START RSYNC..."
                rsync -rav --delete ./src/ \
                ubuntu@$PROD_HOST:/home/ubuntu/prod.kelasdevops.xyz/ \
                --exclude=.env \
                --exclude=storage \
                --exclude=.git
                '''
            }
        }
    }
}