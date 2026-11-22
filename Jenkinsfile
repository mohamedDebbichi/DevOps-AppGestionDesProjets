pipeline {
    agent any
    
    triggers {
        cron('H H/10 * * *')
    }

    tools {
        jdk 'JDK-21'
        maven 'Maven-3.8.7'
    }

    stages {
        stage('Checkout') {
            steps { 
                checkout scm 
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                dir('backend') {
                    script {
                        def scannerHome = tool 'SonarQubeScanner'
                        withSonarQubeEnv('SonarQube') {
                            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=DevOps-AppGestionDesProjets -Dsonar.projectName=DevOps-AppGestionDesProjets -Dsonar.sources=src"
                        }
                    }
                }
            }
        }
        
        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
        
        stage('Test Backend') {
            steps {
                dir('backend') {
                    sh 'mvn test -DskipTests'
                }
            }
        }
        
        /**
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
        */
        
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
            }
        }
    }
}