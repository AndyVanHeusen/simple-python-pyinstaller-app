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
                sh 'pipenv install --dev' // set up that this is also a dev environment
                sh 'pipenv run pytest --verbose --junit-xml test-reports/results.xml sources/test_calc.py --html=test-reports/report.html --cov-report html:cov-reports --cov=sources/ sources/' //run pytest in our pipenv
            }
            post {
                always {
                    junit 'test-reports/results.xml' // always publish the results of the test
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'test-reports',
                        reportFiles: 'report.html',
                        reportTitles: "Test Report",
                        reportName: "Test Report"
                    ])
                }
            }
        }
    }
}