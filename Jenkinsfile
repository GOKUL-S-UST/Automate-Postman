pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/US-TECHNOLOGY-INI01-42988/Automate-Postman.git'
            }
        }

        stage('Install Newman & Reporter') {
            steps {
                 sh '''
            npm install newman@5.3.2
            npm install newman-reporter-html --legacy-peer-deps
        '''
            }
        }

        stage('Run API Tests') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        sh '''
                            echo "Creating reports directory..."
                            mkdir -p reports

                            echo "Running Newman tests..."
                            npx newman run collections/performance_collection4.json \
                              -e environments/postman_environment4.json \
                              --env-var "client_secret=$client_secret" \
                              --reporters cli,htmlextra \
                              --reporter-htmlextra-export reports/newman-report.html

                            echo "Reports directory content:"
                            ls -l reports
                        '''
                    }
                }
            }
        }

        stage('Archive Report') {
            steps {
                sh 'ls -l reports || echo "No report generated!"'
                archiveArtifacts artifacts: 'reports/newman-report.html', fingerprint: true
            }
        }
    }
}
