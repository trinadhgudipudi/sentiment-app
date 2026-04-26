pipeline {
    agent any

    environment {
        // Define environment variables if needed
        PYTHON = "python3"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/trinadhgudipudi/sentiment-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh "${PYTHON} -m pip install --upgrade pip"
                sh "${PYTHON} -m pip install -r requirements.txt"
            }
        }

        stage('Run Tests') {
            steps {
                // Run pytest and generate JUnit XML report
                sh "pytest --junitxml=results.xml || true"
            }
            post {
                always {
                    // Archive test results so they show up in Jenkins
                    junit 'results.xml'
                }
            }
        }

        stage('Run Flask App') {
            steps {
                // Run Flask app in background
                sh "${PYTHON} app.py &"
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

