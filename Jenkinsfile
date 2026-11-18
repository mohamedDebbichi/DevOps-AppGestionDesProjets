pipeline {
    agent any

    tools {
        jdk 'JDK-21'
        maven 'Maven-3.8.7'
    }

    stages {
        stage('Compile') {
            steps {
                dir('backend') {
                    sh 'mvn clean compile'
                }
            }
        }
        
        stage('Test') {
            steps {
                dir('backend') {
                    // Skip tests because the database is not running in this pipeline
                    sh 'mvn test -DskipTests'
                }
            }
        }
        
        stage('Package') {
            steps {
                dir('backend') {
                    sh 'mvn package -DskipTests'
                }
            }
        }
    }
}
