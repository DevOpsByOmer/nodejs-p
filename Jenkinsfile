pipeline {
    agent any
     
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/DevOpsByOmer/nodejs-p.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'ssh user@your-bare-server "cd /path/to/deployment && git pull origin master && npm install && pm2 restart your-app"'
                }
            }
        }
    }
}
