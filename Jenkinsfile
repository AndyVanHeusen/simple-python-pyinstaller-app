pipeline {
    agent any

    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '7', numToKeepStr: '10')
        disableConcurrentBuilds()
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'pip install pytest-html'
                sh 'py.test --verbose --junit-xml test-reports/results.xml --html=test-reports/report.html sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
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
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
    post {
    always {
      //recordIssues enabledForFailure: true, tools: [[pattern: '**/codenarc/main.xml', tool: [$class: 'CodeNArc']]]
      junit 'test-results/**/*.xml'
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
