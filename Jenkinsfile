pipeline {
    agent any

    environment {
        COMPOSER_HOME = "${WORKSPACE}/.composer"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    sh '''
                        # Jika folder src belum ada, clone repo
                        if [ ! -d src ]; then
                            git clone -b main git@github.com:nanditaprihj/devops-laravel.git src
                        else
                            cd src
                            git fetch origin
                            git reset --hard origin/main
                        fi

                        # Atasi error "dubious ownership" di Git
                        git config --global --add safe.directory ${WORKSPACE}/src
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Gunakan Docker composer untuk install dependencies
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
                echo 'Ini adalah test'
            }
        }

        stage('Deploy to Debian') {
            steps {
                // Gunakan SSH Credentials dari Jenkins (ID: jenkins-ssh-key)
                sshagent(['jenkins-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no nandita@192.168.100.10 << EOF
                            cd /home/nandita/prod.kelasdevops.xyz
                            mkdir -p prod
                            rm -rf prod/*
                            cp -r ${WORKSPACE}/src/* prod/
                            cd prod
                            composer install --no-dev --optimize-autoloader
                            php artisan migrate --force
                        EOF
                    """
                }
            }
        }
    }
}