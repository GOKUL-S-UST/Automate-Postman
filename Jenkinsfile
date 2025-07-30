pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/GOKUL-S-UST/Automate-Postman.git'
            }
        }

        stage('Install Newman') {
            steps {
                sh 'npm install newman@5.3.2'
            }
        }

        stage('Run API Tests') {
    steps {
        catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
            sh '''
                mkdir -p reports
                npm install -g newman newman-reporter-htmlextra@1.23.1
                npx newman run collections/performance_collection4.json \
                  -e environments/postman_environment4.json \
                  --insecure \
                  --env-var "client_secret=$client_secret" \
                  --reporters cli,htmlextra \
                  --reporter-htmlextra-export reports/newman-report.html
            '''
        }
    }
}

        stage('Archive Report') {
            steps {
                archiveArtifacts artifacts: 'reports/newman-report.html'
            }
        }
    }
}
