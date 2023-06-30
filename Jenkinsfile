pipeline {
    agent any

    environment {
        mavenhome = tool 'jenkins-maven'
        imageName = "cicddemo"
        registryCredentials = "dockerhubcred"
        registry = ""
        dockerImage = ""
    }

    tools {
        jdk 'jenkins-jdk'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/salehmajd/cicddemo.git'
            }
        }

        stage('Install dependencies') {
            steps {
                bat "mvn clean -DskipTests install"
            }
        }

        stage('Test') {
            steps {
                bat "mvn test"
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build imageName
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry(registry, registryCredentials) {
                        dockerImage.push()
                    }
                }
            }
        }


    }
}