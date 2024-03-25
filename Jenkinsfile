pipeline {
    agent any
 
    tools {nodejs "node"}
 
    stages {
 
        stage('Cloning Git') {
            steps {
                git 'https://github.com/Murod1989/chitchat1.git'
            }
        }
 
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
 
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
}