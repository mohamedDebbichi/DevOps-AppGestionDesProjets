pipeline {
    agent any

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
                    // This runs the tests, but forces Spring Boot to use an H2 in-memory database instead of MySQL!
                    sh 'mvn test -Dspring.datasource.url=jdbc:h2:mem:testdb -Dspring.datasource.driver-class-name=org.h2.Driver'
                }
            }
        }
        
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    // Installs Node.js dependencies and builds the Angular app
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
        
        stage('Archive') {
            steps {
                // Saves the generated .jar file so you can download it later
                archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
            }
        }
    }
}
