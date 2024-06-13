pipeline {
    agent any

    environment {
        USER_CREDENTIALS = credentials('docker-hub')
    }

    stages {
        stage('Checkout') {
            steps {
                //checkout([$class: 'GitSCM',
                //        doGenerateSubmoduleConfigurations: false,
                //        extensions: [],
                //        gitTool: 'Default',
                //        submoduleCfg: [],
                //        userRemoteConfigs: [[url: 'https://github.com/etitovets/http_server.git']]
                //])

                git 'https://github.com/etitovets/http_server'
                sh "ls -la"
            }
        }
        stage('Test') {
            steps {
                sh "go test -v ./..."
            }
        }
        stage('Build') {
            steps {
                sh "go build -o http-server -v ./..."
                archiveArtifacts artifacts: 'http-server', followSymlinks: false
                //sh "docker build -t http-server ."
            }
        }
        stage('Create Image'){
            steps{
               git 'https://github.com/etitovets/http_server'
               sh 'docker build -t zipp/http-server:jenkins .'
               sh 'docker login -u $USER_CREDENTIALS_USR -p $USER_CREDENTIALS_PSW'
               sh 'docker push zipp/http-server:jenkins'

            }
        }
    }
    post {
        success{
            cleanWs()
        }
        failure {
            cleanWs()
        }
        cleanup{
            cleanWs()
        }
    }
}
