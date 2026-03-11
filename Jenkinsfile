node {
    checkout scm

    stage("Build") {
        docker.image('shippingdocker/php-composer:7.4').inside {
            sh '''
            cd src
            rm -f composer.lock
            composer install --ignore-platform-reqs
            '''
        }
    }

    stage("Testing") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Pipeline testing berhasil"'
        }
    }
}
