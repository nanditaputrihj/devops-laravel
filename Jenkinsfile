pipeline {
    agent any

    environment {
        COMPOSER_HOME = "${WORKSPACE}/.composer"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[
                        url: 'git@github.com:nanditaputrihj/devops-laravel.git'
                    ]]
                ])
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.image('composer:2').inside {
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
                script {
                    // Deploy pakai SSH key yang ada di /root/.ssh/id_rsa
                    sh '''
                        ssh -i /root/.ssh/id_rsa -o StrictHostKeyChecking=no nandita@192.168.100.10 << 'EOF'
                            cd /home/nandita/prod.kelasdevops.xyz
                            mkdir -p prod
                            rm -rf prod/*
                            cp -r src/* prod/
                            cd prod
                            composer install --no-dev --optimize-autoloader
                            php artisan migrate --force
                        EOF
                    '''
                }
            }
        }
    }
}