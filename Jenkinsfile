node {
    checkout scm

    stage("Build") {
        docker.image('php:8.2-cli').inside {
            sh 'cd src && rm -f composer.lock'
            sh 'cd src && composer install'
        }
    }

    stage("Testing") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }
}
