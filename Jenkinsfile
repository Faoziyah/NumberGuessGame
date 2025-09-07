pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'jdk17'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Faoziyah/NumberGuessGame.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=NumberGuessGame'
                }
            }
        }
        stage('Publish to Nexus') {
            steps {
                sh 'mvn deploy'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat7(credentialsId: 'tomcat-cred', path: '', url: 'http://3.128.179.181:8080')],
                       contextPath: 'NumberGuessGame',
                       war: '**/target/*.war'
            }
        }
    }
}
