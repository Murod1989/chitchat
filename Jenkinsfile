pipeline {

    environment {
        dockerregistry = 'https://registry.hub.docker.com'
        dockerhuburl = "Murod1989/chitchat1.git"
        githuburl = "Murod1989/chitchat1.git"
        dockerhubcrd = 'murod89'
    }
    agent any
 
    tools {nodejs "node"}
 
    stages {
 
        stage('Cloning Git') {
            steps {
                git 'https://github.com/' + githuburl
            }
        }
 
        stage('Install Node.js dependencies') {
            steps {
                sh 'npm install'
            }
        }
 
        stage('Test App') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build image'){
            steps{
                script {
                    dockerImage = docker.build(dockerhuburl + ":$BUILD_NUMBER")
                }
            }
        }

        stage('Test image'){
            steps {
                sh 'docker run -i ' + dockerhuburl + ':$BUILD_NUMBER npm test'
            }
        }

        stage('Deploy image'){
            steps{
                script{
                    docker.withRegistry(dockerregistry, dockerhubcrd) {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Remove image'){
            steps{
                sh "docker rmi $dockerhuburl:$BUILD_NUMBER"
            }
        }

        stage('Deploy k8s'){
            steps{
                kubernetesDeploy(
                    kubeconfigId: 'k8s',
                    configs: 'k8s.yaml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}