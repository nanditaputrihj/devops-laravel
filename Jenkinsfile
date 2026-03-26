node {
    checkout scm

    stage("Build") {
        docker.image('composer:2').inside('-u root --entrypoint=""') {
            sh '''
            git config --global --add safe.directory /var/jenkins_home/workspace/laravel-dev
            cd src
            composer install --no-dev --optimize-autoloader
            '''
        }
    }

    stage("Testing") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

    stage("Deploy to Debian") {
        sh '''
        ssh nandita@192.168.100.10 "
            cd /home/nandita/prod.kelasdevops.xyz &&

            mkdir -p prod &&
            rm -rf prod/* &&

            cp -r src/* prod/ &&

            cd prod &&

            composer install --no-dev --optimize-autoloader &&
            php artisan migrate --force
        "
        '''
    }
}