pipeline {
    agent any
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '7', numToKeepStr: '10')
        disableConcurrentBuilds()
    }
    stages {
        stage('Build') {
            steps {
                sh 'pipenv --three ' // install virtual environment
                sh 'pipenv install --dev' //
                sh 'pipenv run pytest --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                    // publishHTML(target: [
                    //     allowMissing: false,
                    //     alwaysLinkToLastBuild: false,
                    //     keepAll: true
                    // ])
                }
            }
        }
    }
}