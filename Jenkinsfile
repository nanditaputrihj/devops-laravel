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

    stage("Deploy Local") {
        sh '''
        mkdir -p /home/nandita/prod.kelasdevops.xyz
        cp -r src/* /home/nandita/prod.kelasdevops.xyz/
        '''
    }
}
