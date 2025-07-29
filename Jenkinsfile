pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/GOKUL-S-UST/Automate-Postman.git'
            }
            }
        }

        stage('Install Newman') {
            steps {
                sh 'npm install -g newman'
            }
        }

        stage('Run API Tests') {
            steps {
                sh '''
                    newman run collections/performance_collection4.json \
                      -e environments/postman_environments4.json \
                      --env-var "client_secret=$client_secret"
                      --reporters cli,html \
                      --reporter-html-export newman-report.html
                '''
            }
        }

        stage('Archive Report') {
            steps {
                archiveArtifacts artifacts: 'newman-report.html', onlyIfSuccessful: true
            }
        }
    }

