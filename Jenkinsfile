pipeline {
    agent any
    
    tools{
        jdk 'jdk17'
        nodejs 'node16'
        
    }
    
    environment{
        SCANNER_HOME= tool 'Sonar7'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jaiswaladi246/fullstack-bank.git'
                echo 'check completed succesfully'
            }
        }
        
        
        
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
                echo 'check trivy scan done'
            }
        }
        
        stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar7') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank "
                    echo 'code Analysis complted'
                }
            }
        }
        
        
         stage('Install Dependencies') {
            steps {
                sh "npm install"
                echo 'installed dependencies'
            }
        }
        
        stage('Backend') {
            steps {
                dir('/root/.jenkins/workspace/Bank/app/backend') {
                    sh "npm install"
                    echo 'done'
                }
            }
        }
        
        stage('frontend') {
            steps {
                dir('/root/.jenkins/workspace/Bank/app/frontend') {
                    sh "npm install"
                }
            }
        }
        
        stage('Deploy to Conatiner') {
            steps {
                sh "npm run compose:up -d"
            }
        }
    }
}
