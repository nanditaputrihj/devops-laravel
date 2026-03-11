node {
    checkout scm
    // deploy env dev
    stage("Build"){
        docker.image('shippingdocker/php-composer:7.4').inside('-u root') {
            sh 'cd src && rm -f composer.lock'
            sh 'cd src && composer install'
        }
    }
    // Testing
    docker.image('ubuntu').inside('-u root') {
       sh 'echo "Ini adalah test"'
    }
}

