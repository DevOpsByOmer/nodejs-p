def COLOR_MAP = [
    SUCCESS: 'good', 
    FAILURE: 'danger',
]

pipeline {
    agent any

    environment {
        SONAR_URL = "http://192.168.33.10:9000"
        JAVA_HOME = tool('JDK')  
    }

    tools {
        nodejs "NODE" 
        jdk "JDK"
        maven "MVN"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Cache npm Dependencies') {
            steps {
                script {
                    // Check if cache exists
                    if (fileExists('GitHubDir')) {
                        echo 'Restoring npm dependencies from cache...'
                        unstash name: 'GitHubDir'
                    } else {
                        echo 'No node_modules found in cache.'
                    }
                }
            }
        }

        stage('Fetching the Code') {
            steps {
                sh "git clone -b main https://github.com/DevOpsByOmer/nodejs-p.git"
            }
        }


        stage('Install Dependencies') {
            steps {
                script {
                    sh 'cd GitHub- && npm install'
                }
            }
        }
         stage('Sonar Code Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar/bin/sonar-scanner -X -Dsonar.projectKey=nodeapp \
                        -Dsonar.projectName=nodeapp \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=/var/lib/jenkins/workspace/Nodejs_project/nodejs-p/src/'''
                }
            }
        }
    
         stage('Stash npm Dependencies') {
            steps {
                script {
                    echo 'Stashing npm dependencies for future builds...'
                    stash name: 'GitHubDir', includes: '/var/lib/jenkins/workspace/Nodejs_project/nodejs-p/node_module/**', allowEmpty: true
                }
            }
        }
        stage('Deploy to Nginx') {
            steps {
                script {
                    echo 'Deploying to Nginx...'
                    sh 'rsync -a nodejs-p/build/ /var/www/html/'
                }
            }
        }
       
     post {
        always {
            echo 'Slack Notification.'
            slackSend channel: '#jenkinscicd',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n",
                botUser: true,
                tokenCredentialId: 'slacktoken',
                notifyCommitters: false
        }
    }
    }
}


