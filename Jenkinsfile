pipeline {

    agent any

    environment {
        registry = "patrickguha/jenkinshelloworld"
    }

    stages {
       
        stage('Cloning Git') {
            steps {
                git 'https://github.com/ironspur5/cloud-native-jenkins-hello-world'
            }
        }

        stage('Build App') {
            steps {
                sh 'go build'
            }
        }

        stage('Test App') {
            steps {
                sh 'go test -v'
            }
        }

        stage('Building Image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Publish Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                    dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            } 
        }

    }

    
}
