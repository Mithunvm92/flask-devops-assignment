pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'python3 -m venv .venv'
                sh '.venv/bin/pip install --upgrade pip'
                sh '.venv/bin/pip install -r requirements.txt'
                sh '.venv/bin/pip install pytest'
            }
        }

        stage('Test') {
            steps {
                sh '.venv/bin/pytest -v'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'mkdir -p staging'
                sh 'cp app.py staging/'
                sh 'cp -r templates staging/'
                sh 'cp requirements.txt staging/'
                echo 'Flask application deployed to staging successfully.'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully.'
        }
        failure {
            echo 'CI/CD Pipeline failed.'
        }
    }
}
