pipeline {
    agent any

    environment {
        COMPOSER_HOME = "${WORKSPACE}/.composer"
        SSH_KEY_PATH = "/root/.ssh/id_rsa"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    sh '''
                        # Pastikan ssh-agent jalan
                        eval $(ssh-agent -s)
                        ssh-add ${SSH_KEY_PATH}

                        # Clone repo jika belum ada, kalau sudah ada fetch update
                        if [ ! -d src ]; then
                            git clone -b main git@github.com:nanditaputrihj/devops-laravel.git src
                        else
                            cd src
                            git fetch origin
                            git reset --hard origin/main
                        fi
                    '''
                }
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
                    sh """
                        ssh -i ${SSH_KEY_PATH} -o StrictHostKeyChecking=no nandita@192.168.100.10 << 'EOF'
                            cd /home/nandita/prod.kelasdevops.xyz
                            mkdir -p prod
                            rm -rf prod/*
                            cp -r src/* prod/
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