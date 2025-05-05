pipeline {
    agent any

    tools {
        maven 'Maven3'     // Name should match Global Tool Config
        jdk 'Java11'       // Name should match Global Tool Config
    }

    environment {
        CHROME_BIN = '/usr/bin/google-chrome'
        CHROMEDRIVER_BIN = '/usr/local/bin/chromedriver'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/TanujaMarni/automatic-testing/tree/main/Selenium-Automatic-Testing-main'
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
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }
    }

    post {
        success {
            echo '✅ Build and tests passed successfully.'
        }
        failure {
            echo '❌ Build or tests failed.'
        }
    }
}
