pipeline {
    agent any

    tools {
        maven 'Maven3'    // Name defined in Jenkins -> Global Tool Configuration
        jdk 'Java11'      // Name defined in Jenkins -> Global Tool Configuration
    }

    environment {
        CHROME_BIN = '/usr/bin/google-chrome'
        CHROMEDRIVER_BIN = '/usr/local/bin/chromedriver'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git'
            }
        }

        stage('Install Chrome & Chromedriver') {
            steps {
                sh '''
                    sudo apt-get update
                    sudo apt-get install -y google-chrome-stable unzip xvfb
                    wget https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_linux64.zip
                    unzip chromedriver_linux64.zip
                    sudo mv chromedriver /usr/local/bin/
                    sudo chmod +x /usr/local/bin/chromedriver
                '''
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Publish Test Report') {
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Tests passed successfully!'
        }
        failure {
            echo 'Some tests failed.'
        }
    }
}
