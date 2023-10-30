pipeline {
    agent {
        label 'ajtest-node'
    }
    environment {
        SECRET_VALUE = credentials('ajsnyktoken')
    }

    stages {
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Research-Associate-Internship/ajosyulatest.git']])
            }
        }
        stage('build') {
            steps {
                sh 'npm install'
            }
        }
        stage('sast-testing') {
            steps {
                /*script {*/
                    /*withCredentials([string(credentialsId: 'ajsnyktoken', variable: 'SECRET_VALUE')]) {*/
                        snykSecurity failOnIssues: false, projectName: 'juice-shop', snykInstallation: 'SnykJ', snykTokenId: '${env.SECRET_VALUE}'
                    /*}*/
                }
        }
        }
        /*stage('run-application') {
            steps {
                sh 'docker run -d -p 80:3000 --name owasp bkimminich/juice-shop'
            }
        }
        stage('dast-testing') {
            steps {
                sh 'cd /usr/share/owasp-zap; ./zap.sh -cmd -quickurl http://3.231.214.33:80 -quickout /home/ubuntu/zap_reports/JENKINS_ZAP_VULNERABILITY_REPORT_${BUILD_ID}.html -quickprogress'
                
            }
        }
        stage('stop-application') {
            steps {
                sh 'docker stop $(docker ps -q)'
                sh 'docker rm $(docker ps -a -q)'
            }
        }*/
    }
    /*post {
        always {
            publishHTML(target: [
                allowMissing:false,
                alwaysLinkToLastBuild:true,
                keepAll:true,
                reportDir:'/home/ubuntu/zap_reports',
                reportFiles: 'JENKINS_ZAP_VULNERABILITY_REPORT_${BUILD_ID}.html',
                reportName: 'OWASP_ZAP_Scan_Report'
                ])
        }
    }*/
