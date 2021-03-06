/**
 * How to doing a NodeJS pipeline in Jenkins
 * https://medium.com/@gustavo.guss/jenkins-starting-with-pipeline-doing-a-node-js-test-72c6057b67d4
 * 
 * How to add stage to build and sending an image to registry with Docker
 * https://medium.com/@gustavo.guss/jenkins-building-docker-image-and-sending-to-registry-64b84ea45ee9
 */
pipeline {
    
  
    environment {
        registry = "lorenzorangelsuarez/test-nodejs-pipeline"
        registryCredential = 'dockerHubID'
        dockerImage = ''
    } 
    agent any
    
    tools {nodejs "nodejs"}
    
    stages {
        
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'lorenzo.globant', url: 'https://github.globant.com/Mexico-Talent-Pool/tnn-marketplace-ui.git']]])
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                //sh 'npm test'
            }
        }
        
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        } 
        /*
        stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }*/
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}

