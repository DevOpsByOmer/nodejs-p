pipeline {
    agent any
     
    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/DevOpsByOmer/nodejs-p.git'
            }
        }

        stage('install dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('build') {
            steps {
                script {
                   sh 'npm run build'
            }
        }
    }
}
}
