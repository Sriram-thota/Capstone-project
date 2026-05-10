pipeline {
    agent any
    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Sriram-thota/Capstone-project.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }
        stage('Run API Tests') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    bat 'python -m pytest tests/api -v --alluredir=reports/allure-results'
                }
            }
        }
        stage('Run UI Tests') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    bat 'python -m pytest tests/ui -v --alluredir=reports/allure-results'
                }
            }
        }
        stage('Run E2E Tests') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    bat 'python -m pytest tests/e2e -v --alluredir=reports/allure-results'
                }
            }
        }
        stage('Generate Allure Report') {
            steps {
                allure(
                    includeProperties: false,
                    jdk: '',
                    results: [[path: 'reports/allure-results']]
                )
            }
        }
    }
    post {
        success {
            echo 'All tests passed successfully.'
        }
        unstable {
            echo 'Some tests failed. Build marked as UNSTABLE.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
