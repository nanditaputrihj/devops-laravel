node {
    checkout scm

    stage("Build") {
        docker.image('shippingdocker/php-composer:7.4').inside {
            sh '''
            cd src
            rm -f composer.lock
            composer install --ignore-platform-reqs --no-scripts
            '''
        }
    }

    stage("Testing") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Pipeline testing berhasil 🚀"'
        }
    }

    // deploy env prod
    stage("Deploy Production") {
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
            sshagent (credentials: ['ssh-prod']) {
                sh 'mkdir -p ~/.ssh'
                sh 'ssh-keyscan -H "$PROD_HOST" >> ~/.ssh/known_hosts'
                sh '''
                rsync -rav --delete ./src/ \
                nandita@$PROD_HOST:/home/ubuntu/prod.kelasdevops.xyz/ \
                --exclude=.env --exclude=storage --exclude=.git
                '''
            }
        }
    }
}

