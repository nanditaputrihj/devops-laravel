pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/nanditaputrihj/devops-laravel.git'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t laravel-app .'
            }
        }

    }
}