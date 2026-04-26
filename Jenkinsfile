pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/trinadhgudipudi/sentiment-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m pip install --upgrade pip'
                sh 'python3 -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                // Run pytest and generate JUnit XML report
                sh 'pytest --junitxml=results.xml || true'
            }
            post {
                always {
                    // Publish test results in Jenkins
                    junit 'results.xml'
                }
            }
        }
    }

    post {
        always {
            // Archive logs or artifacts if needed
            archiveArtifacts artifacts: '**/*.log', allowEmptyArchive: true
        }
    }
}
