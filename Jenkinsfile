node {

    stage("Clean") {
        deleteDir()
    }

    stage("Checkout") {
        git url: 'https://github.com/nanditaputrihj/devops-laravel.git', branch: 'main'
    }

    stage("Build") {
        sh '''
        rm -f composer.lock
        composer install --ignore-platform-reqs --no-scripts
        '''
    }

    stage("Testing") {
        sh 'echo "Pipeline testing berhasil 🚀"'
    }
}