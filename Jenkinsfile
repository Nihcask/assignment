pipeline {
    agent any
    tools{
        maven 'maven'
    }
    environment {
    DOCKERHUB_CREDENTIALS = credentials('docker')
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Nihcask/assignment.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t sachink0912/kubernetes:$BUILD_NUMBER .'
                }
            }
        }
        stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u sachink0912 -p ${dockerhubpwd}'
                        
                    }
                    sh 'docker push sachink0912/kubernetes:$BUILD_NUMBER'
                }
            }
        }
        stage('Deploy to K8s'){
            agent {
                label "k8s"
            }
            steps{
                sh 'helm install assv2 /home/ubuntu/assv2/'
            }
        }
    }
}
