node {
    checkout scm
    // deploy env dev
    stage("Build"){
        docker.image('composer:2.7')
            sh 'cd src && rm -f composer.lock'
            sh 'cd src && composer install'
        }
    }
    // Testing
    docker.image('ubuntu').inside('-u root') {
       sh 'echo "Ini adalah test"'
    }
}

