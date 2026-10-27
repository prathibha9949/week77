
pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                echo 'Source code retrieved from GitHub'
            }
        }

        stage('Check Docker') {
            steps {
                bat 'docker version'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t mypythonflaskapp:%BUILD_NUMBER% .'
            }
        }

        stage('Verify Image') {
            steps {
                bat 'docker image inspect mypythonflaskapp:%BUILD_NUMBER%'
            }
        }

        stage('Stop Old Container') {
            steps {
                script {
                    bat(
                        returnStatus: true,
                        script: 'docker rm -f flask-container'
                    )
                }
            }
        }

        stage('Deploy Container') {
            steps {
                bat 'docker run -d --name flask-container -p 5000:5000 mypythonflaskapp:%BUILD_NUMBER%'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker ps'
                echo 'Application deployed on port 5000'
            }
        }
    }
}
