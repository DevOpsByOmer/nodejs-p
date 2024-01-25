pipeline {
    agent any

    environment {
        NODE_VERSION = 'NODE'
    }

    tools {
        nodejs "NODE" 
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Use npm to install dependencies
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run your Node.js tests
                    sh 'npm test'
                }
            }
        }
    
        stage('deploy') {
          steps {
              script {
                    // Add deployment steps for the main branch
                    echo 'Deploying to production...'
                }
            }
        }
        }
}
    

