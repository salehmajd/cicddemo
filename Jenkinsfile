pipeline {
    agent any

    environment {
        mavenhome = tool 'jenkins-maven'
        imageName = "cicddemo"
        registryCredentials = "nexus"
        registry = "localhost:8083/"
        dockerImage = ""
    }

    tools {
        jdk 'jenkins-jdk'
    }

    stages {
        stage('Build') {
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
                    docker.withRegistry("http://" + registry, registryCredentials) {
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}