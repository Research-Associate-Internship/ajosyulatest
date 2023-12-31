// Code by Abhishek Josyula

pipeline {
    agent {
        label 'ajtest-node'
    }
    /*environment {
        SNYK_TOKEN = credentials('snykajtoken')
    }*/
    stages {
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Research-Associate-Internship/ajosyulatest.git']])
            }
        }
        stage('build') {
            steps {
                sh 'sudo npm install'
            }
        }
        // This is Jenkins generated Snyk code
        /*stage('sast-testing') { 
            steps {
                snykSecurity failOnIssues: false, projectName: 'juice-shop', snykInstallation: 'SnykJ', snykTokenId: '${env.SNYK_TOKEN}'
                }
                
        }*/
        stage('sast-testing') {
            steps {
                withCredentials([string(credentialsId: 'snykajtoken', variable: 'SNYK_TOKEN')]) {
                sh """
                snyk auth $SNYK_TOKEN
                snyk test --json | snyk-to-html > /home/ubuntu/security_reports/Snyk_Report_${BUILD_ID}.html
                """
                }
            }
            }
        stage('run-application') {
            steps {
                sh 'docker run -d -p 80:3000 --name owasp bkimminich/juice-shop'
            }
        }
        stage('dast-testing') {
            steps {
                sh 'cd /usr/share/owasp-zap; ./zap.sh -cmd -quickurl http://3.80.11.52//:80 -quickout /home/ubuntu/security_reports/JENKINS_ZAP_VULNERABILITY_REPORT_${BUILD_ID}.html -quickprogress'
                
            }
        }
        stage('stop-application') {
            steps {
                sh 'docker stop $(docker ps -q)'
                sh 'docker rm $(docker ps -a -q)'
            }
        }
    }
    post {
        always {
            publishHTML(target: [
                allowMissing:false,
                alwaysLinkToLastBuild:true,
                keepAll:true,
                reportDir:'/home/ubuntu/security_reports',
                reportFiles: 'JENKINS_ZAP_VULNERABILITY_REPORT_${BUILD_ID}.html, Snyk_Report_${BUILD_ID}.html',
                reportName: 'Vulnerability Reports'
                ])
        }
    }
}
