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
                sh 'pipenv run pytest --verbose --junit-xml test-reports/results.xml sources/test_calc.py --html=test-reports/report.html --cov-report html:cov-reports --cov-report xml:cov-reports/cov.xml --cov=sources/ sources/' //run pytest in our pipenv
            }
            post {
                always {
                    
                    junit 'test-reports/results.xml' // always publish the results of the test

                    cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/build/reports/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false

                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'test-reports',
                        reportFiles: 'report.html',
                        reportTitles: "Test Report",
                        reportName: "Test Report"
                    ])
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'cov-reports',
                        reportFiles: 'index.html',
                        reportTitles: "Coverage Report",
                        reportName: "Coverage Report"
                    ])
                }
            }
        }
    }
}