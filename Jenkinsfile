pipeline {
    agent any

    environment {
        COMPOSER_HOME = "${WORKSPACE}/.composer"
    }

    stages {
        stage('Checkout') {
            steps {
                // Pakai SSH Credentials Jenkins
                sshagent(['jenkins-ssh-key']) {
                    script {
                        sh '''
                            # Jika folder src belum ada, clone repo via SSH port 443
                            if [ ! -d src ]; then
                                git clone -b main ssh://git@ssh.github.com:443/nanditaprihj/devops-laravel.git src
                            else
                                cd src
                                git fetch origin
                                git reset --hard origin/main
                            fi

                            # Atasi error "dubious ownership"
                            git config --global --add safe.directory ${WORKSPACE}/src
                        '''
                    }
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
                echo 'Ini adalah stage Testing (dummy)'
            }
        }

        stage('Deploy to Debian') {
    steps {
        sshagent(['jenkins-ssh-key']) {
            sh """
                # Buat folder di server (kalau belum ada)
                ssh -o StrictHostKeyChecking=no nandita@192.168.100.10 "mkdir -p /home/nandita/prod.kelasdevops.xyz/prod"

                # Copy file dari Jenkins ke server
                scp -o StrictHostKeyChecking=no -r ${WORKSPACE}/src/* nandita@192.168.100.10:/home/nandita/prod.kelasdevops.xyz/prod/

                # Jalankan command di server
                ssh -o StrictHostKeyChecking=no nandita@192.168.100.10 << EOF
                    cd /home/nandita/prod.kelasdevops.xyz/prod
                    composer install --no-dev --optimize-autoloader
                    php artisan migrate --force
                EOF
            """
        }
    }
}