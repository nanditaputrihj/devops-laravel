pipeline {
    agent any

    environment {
        COMPOSER_HOME = "${WORKSPACE}/.composer"
    }

    stages {
        stage('Checkout') {
            steps {
                sshagent(['jenkins-ssh-key']) {
                    sh '''
                        if [ ! -d src ]; then
                            git clone -b main ssh://git@ssh.github.com:443/nanditaputrihj/devops-laravel.git src
                        else
                            cd src
                            git fetch origin
                            git reset --hard origin/main
                        fi

                        git config --global --add safe.directory ${WORKSPACE}
                        git config --global --add safe.directory ${WORKSPACE}/src
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.image('composer:2').inside('--entrypoint=""') {
                        sh '''
                            cd src
                            composer install --no-dev --optimize-autoloader
                            php artisan package:discover --ansi
                        '''
                    }
                }
            }
        }

        stage('Testing') {
            steps {
                echo 'Testing stage'
            }
        }

        stage('Deploy to Debian') {
    steps {
        sshagent(['nandita']) {
            sh '''
            ssh -o StrictHostKeyChecking=no nandita@192.168.100.10 "mkdir -p /home/nandita/prod.kelasdevops.xyz/prod"

            scp -o StrictHostKeyChecking=no -r src/* nandita@192.168.100.10:/home/nandita/prod.kelasdevops.xyz/prod/

            ssh -o StrictHostKeyChecking=no nandita@192.168.100.10 "
            chown -R nandita:www-data /home/nandita/prod.kelasdevops.xyz/prod &&
            chmod -R 775 /home/nandita/prod.kelasdevops.xyz/prod &&
            chmod -R 775 /home/nandita/prod.kelasdevops.xyz/prod/storage &&
            chmod -R 775 /home/nandita/prod.kelasdevops.xyz/prod/bootstrap/cache &&
            chmod -R 775 /home/nandita/prod.kelasdevops.xyz/prod/database &&
            chmod 664 /home/nandita/prod.kelasdevops.xyz/prod/database/database.sqlite
            "
            '''
        }
    }
}
    }
}