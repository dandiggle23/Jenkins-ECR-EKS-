pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        registry = '642733809371.dkr.ecr.us-east-1.amazonaws.com/docker-repo'
    }
    stages {
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/dandiggle23/Jenkins-ECR-EKS-.git']])
            }
        }

        stage('BuildJar') {
            steps {
               sh 'mvn clean install'
            }
        }
        stage('Build Docker image') {
            steps {
               script {
                   docker.build registry
               }
            }
        }
        stage('Push to ECR') {
            steps {
               sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 642733809371.dkr.ecr.us-east-1.amazonaws.com'
               sh 'docker push 642733809371.dkr.ecr.us-east-1.amazonaws.com/docker-repo:latest'
            }
        }
        stage('Deploy to K8S') {
            steps {
               withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                   sh 'kubectl apply -f eks-deploy-k8s.yaml'
             }
            }
        }
    }
}
