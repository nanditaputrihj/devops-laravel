node {
    checkout scm

    // Build
    stage("Build") {
        docker.image('composer:2.7').inside {
            sh 'cd src && rm -f composer.lock'
            sh 'cd src && composer install'
        }
    }

    // Testing
    stage("Testing") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }
}
