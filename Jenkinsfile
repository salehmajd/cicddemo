pipeline {
    agent any

    environment {
        mavenhome = tool 'jenkins-maven'
        imageName = "cicddemo"
        registryCredentials = "nexus"
        registry = ""
        dockerImage = "http://localhost:9001/repository/nexus-docker/"
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
                bat "mvn jar:jar deploy:deploy"
            }
        }
    }
}