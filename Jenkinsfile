node {

    stage("Clean") {
        deleteDir()
    }

    stage("Checkout") {
        checkout scm
    }

    stage("Build") {
        docker.image('shippingdocker/php-composer:7.4').inside('-u root') {
            sh '''
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
}