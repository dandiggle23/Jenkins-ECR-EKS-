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
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/akannan1087/springboot-app']])
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
        
    }
}
