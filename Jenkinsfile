pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'pipenv --three ' // install virtual environment
                sh 'pipenv install --dev' //
                sh 'pytest --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        }
    }
}